# Gradle Boilerplate Project

## How to create Gradle Project with git Source Repository?

Here we will going to use grgit gradle plugin let's follow the Steps




### build.gradle
Add below codes into **build.gradle**

```
// All Git repository List 
def repositoryMap =  [
        "java-common" : "https://github.com/hmtmcse/java-common.git",
]


// Clone Repository Task
task cloneRepositories {
    doLast {
        repositoryMap.each { name, url ->
            println("------------------------------------------------------------------------------------------")
            def destination = file("ex-plugins/${name}")
            try{
                println("Cloning Project ${name}")
                org.ajoberstar.grgit.Grgit.clone(dir: destination, uri: url)
            }catch(Exception e){
                println(e.getMessage())
            }
            println("------------------------------------------------------------------------------------------\n")
        }
    }
}


dependencies {

    // if external source repositories directory is available then add those as dependency
    File exRepositories = file('ex-repositories/')
    if (exRepositories.exists()){
        exRepositories.list().each {
            implementation project(":${it}")
        }
    }
    
    testImplementation group: 'junit', name: 'junit', version: '4.12'
}
```


<br><br>

### settings.gradle 
Add below codes into **settings.gradle**

```
File configFile = file('ex-plugins/')
if (configFile.exists()){
    configFile.list().each {
        include(it)
        project(":${it}").projectDir = file("ex-plugins/${it}")
    }
}
```








<br><br><br><br>

**Reference**
1. Gradle grplugin: https://plugins.gradle.org/plugin/org.ajoberstar.grgit
