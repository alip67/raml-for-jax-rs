## Resource and Response generation control

We have an experimental feature to allow for controlling the generation process.

It's possible to modify class/interface/method generation through usage of RAML 1.0 annotations.
Current annotations are:
``` yaml
annotationTypes:
    types:
        allowedTargets: TypeDeclaration
        properties:
            className?:
                type: string
            implementationClassName?:
                type: string
            usePrimitiveType?:
                type: boolean
            abstract?:
                type: boolean
    resources:
        allowedTargets: [Resource, API]
        properties:
            onResourceClassCreation?: string
            onResourceClassFinish?: string
    methods:
        allowedTargets: [Method, API]
        properties:
            onResourceMethodCreation?: string
            onResourceMethodFinish?: string
            onResponseClassCreation?: string
            onResponseClassFinish?: string
    responses:
        allowedTargets: [Response, API]
        properties:
            onResponseMethodCreation?: string
            onResponseMethodFinish?: string
```

The *Creation methods are fairly early in the process.  Resource creation, for example, handles the resource creation process
 (the recursion, the finding of the methods and such).  This allows you to override the entire process.  The APIs here are not complete,
 but should you want to prevent the generation of a class by returning null, this is an easy place to do it.
  
The *Finish methods are called just before the build part, when raml-to-jaxrs thinks it has finished. This is the one you propably want to override.

As we said, this is experimental (we might change the interface).

The classes that interfaces the allow for interception are [here](src/main/java/org/raml/jaxrs/generator/extension/resources).

The classes/interfaces/methods will be generated by JavaPoet.  Each of the methods should return the *.Builder that was passed to it OR a new
one built by the method (except in the *Creation plugins, which should be passed a null Builder).  

It should be noted that if a plugin method returns null, the class or method will not be generated.