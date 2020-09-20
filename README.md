## Description
Integration of Ktor and Vue the easy way

 [Heroku App](https://infinite-reef-26326.herokuapp.com)

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

      ```
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
3. `heroku container:push web -a infinite-reef-26326`
4. `heroku container:release web -a infinite-reef-26326`

[Heroku App](https://infinite-reef-26326.herokuapp.com)



