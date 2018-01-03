cra-monorepo
============

This is an example of creating a monorepo using create-react-app.

It does not work currently,
See [CRA issue #3676](https://github.com/facebookincubator/create-react-app/issues/3676).

The original issue was found with an ejected CRA. However I am running into issues even before ejecting.

Repo Structure
--------------
    cra-monorepo
    |--- myapp (CRA)
    |    `--- src
    |         `--- components
    |              `--- Home.js
    `--- shared (shared components)
         `--- components
              `--- Profile.js

The `.env` file under myapp has been set up to import source from `./src` and `../shared`:

    NODE_PATH=./src:../shared
 
Issue Before Ejecting
---------------------
```
cd myapp
yarn
yarn start
```

You will see the following error:

    Failed to compile.
    
    ../shared/components/Profile.js
    Module parse failed: Unexpected token (5:16)
    You may need an appropriate loader to handle this file type.
    | class Profile extends React.Component {
    |     render() {
    |         return (<div>Profile</div>);
    |     }
    | }

Basically CRA fails to recognize `Profile.js` as a JSX file.

If you remove the `Profile` component from `myapp/src/App.js`, the app works correctly, demonstrating that local components such as `Home` work perfectly. It's only the components in the `shared` folder that have the problem.

Issue After Ejecting
--------------------
Currently the behavior after ejecting is the same as before ejecting. I am not able to reproduce the original problem. However, once we get this current issue resolved, I will try to reproduce the original issue. For reference, the original issue is that `npm start` outputs the following error:

    /Users/nbhati/projects/myapp-monorepo/myapp-embeds/node_modules/react-dev-utils/ModuleScopePlugin.js:28
            request.descriptionFileRoot.indexOf('/node_modules/') !== -1 ||
                                        ^
    
    TypeError: Cannot read property 'indexOf' of undefined
        at Resolver.resolver.plugin (/Users/nbhati/projects/myapp-monorepo/myapp-embeds/node_modules/react-dev-utils/ModuleScopePlugin.js:28:37)
        at Resolver.applyPluginsAsyncSeriesBailResult1 (/Users/nbhati/projects/myapp-monorepo/myapp-embeds/node_modules/tapable/lib/Tapable.js:256:13)
        at runNormal (/Users/nbhati/projects/myapp-monorepo/myapp-embeds/node_modules/enhanced-resolve/lib/Resolver.js:130:20)
        at Resolver.doResolve (/Users/nbhati/projects/myapp-monorepo/myapp-embeds/node_modules/enhanced-resolve/lib/Resolver.js:116:3)
        at Resolver.<anonymous> (/Users/nbhati/projects/myapp-monorepo/myapp-embeds/node_modules/enhanced-resolve/lib/TryNextPlugin.js:16:12)
        at Resolver.applyPluginsAsyncSeriesBailResult1 (/Users/nbhati/projects/myapp-monorepo/myapp-embeds/node_modules/tapable/lib/Tapable.js:256:13)
        at runNormal (/Users/nbhati/projects/myapp-monorepo/myapp-embeds/node_modules/enhanced-resolve/lib/Resolver.js:130:20)
        at Resolver.doResolve (/Users/nbhati/projects/myapp-monorepo/myapp-embeds/node_modules/enhanced-resolve/lib/Resolver.js:116:3)
        at Resolver.<anonymous> (/Users/nbhati/projects/myapp-monorepo/myapp-embeds/node_modules/enhanced-resolve/lib/FileKindPlugin.js:17:12)
        at Resolver.applyPluginsAsyncSeriesBailResult1 (/Users/nbhati/projects/myapp-monorepo/myapp-embeds/node_modules/tapable/lib/Tapable.js:256:13)
        at runNormal (/Users/nbhati/projects/myapp-monorepo/myapp-embeds/node_modules/enhanced-resolve/lib/Resolver.js:130:20)
        at Resolver.doResolve (/Users/nbhati/projects/myapp-monorepo/myapp-embeds/node_modules/enhanced-resolve/lib/Resolver.js:116:3)
        at Resolver.<anonymous> (/Users/nbhati/projects/myapp-monorepo/myapp-embeds/node_modules/enhanced-resolve/lib/NextPlugin.js:14:12)
        at Resolver.applyPluginsAsyncSeriesBailResult1 (/Users/nbhati/projects/myapp-monorepo/myapp-embeds/node_modules/tapable/lib/Tapable.js:256:13)
        at runAfter (/Users/nbhati/projects/myapp-monorepo/myapp-embeds/node_modules/enhanced-resolve/lib/Resolver.js:152:20)
        at innerCallback (/Users/nbhati/projects/myapp-monorepo/myapp-embeds/node_modules/enhanced-resolve/lib/Resolver.js:146:3)
