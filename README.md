## Description
Integration of Ktor and Vue the easy way

 [Heroku App](https://url-short-srvc.herokuapp.com)

## Requirements
1. Vue 3.0.0 [We need the built files in `dist`]

2. Ktor 1.4.0  

3. ShadowJar dependency 5.0.0 [See below how to add it]

## Steps

1. Build `Vue project` and  copy all contents `[VueProject]\dist`  to `[KtorProjectLocation]\resources\static`

2. Add to `build.gradle.kts`

   ```kotlin
   ...
   plugins {
   	...
   	id("com.github.johnrengelman.shadow") version "5.0.0"
   	...
   }
   ...
   tasks.withType<Jar> {
       manifest {
           attributes(
               mapOf(
                   "Main-Class" to application.mainClassName
               )
           )
       }
   }
   ```

3.  Add `static` route to the `Application`

      ```kotlin
      routing {
        ...
            static("/") {
                resources("static")
                defaultResource("static/index.html")
            }
            ...
      }
      ```

### Run using docker
Create `Dockerfile` in `project root` and use `your_file`.jar (found in `build/libs`)
```dockerfile
FROM openjdk:8-jre-alpine

ENV APPLICATION_USER ktor
RUN adduser -D -g '' $APPLICATION_USER

RUN mkdir /app
RUN chown -R $APPLICATION_USER /app

USER $APPLICATION_USER

COPY ./build/libs/ktor_vue-0.0.1-all.jar /app/ktor_vue-0.0.1-all.jar
WORKDIR /app

CMD ["java", "-server", "-XX:+UnlockExperimentalVMOptions", "-XX:+UseCGroupMemoryLimitForHeap", "-XX:InitialRAMFraction=2", "-XX:MinRAMFraction=2", "-XX:MaxRAMFraction=2", "-XX:+UseG1GC", "-XX:MaxGCPauseMillis=100", "-XX:+UseStringDeduplication", "-jar", "ktor_vue-0.0.1-all.jar"]
```
##### Steps
1. `gradlew build`
2. `docker build -t ktor_vue_app .`
3. `docker run -m512M --cpus 2 -it -p 8080:8090 --rm ktor_vue_app`
4. Open `localhost:8080`

### Run using Intellij IDEA Ultimate
1. Run `main` function
2. Open `localhost:8080`


### Deploy to Heroku
1. `heroku login`
2. `heroku container:login`
3. `heroku container:push web -a url-short-srvc`
4. `heroku container:release web -a url-short-srvc`

[Heroku App](https://url-short-srvc.herokuapp.com)



