cra-monorepo
============

This is an example of creating a monorepo using create-react-app.

It does not work currently,
See [CRA issue #3676](https://github.com/facebookincubator/create-react-app/issues/3676).

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

If I remove the `Profile` component from `myapp/src/App.js`, the app works correctly, demonstrating that local components such as `Home` work perfectly. It's only the components in the `shared` folder that have the problem.

Issue After Ejecting
--------------------
To fix the issue. I have ejected.

- I added an `appShared` export in [paths](https://github.com/nareshbhatia/cra-monorepo/blob/master/myapp/config/paths.js#L50).
- I added `paths.appShared` to the webpack config so that the jsx files are passed through eslint and babel. See [here](https://github.com/nareshbhatia/cra-monorepo/blob/master/myapp/config/webpack.config.dev.js#L125) and [here](https://github.com/nareshbhatia/cra-monorepo/blob/master/myapp/config/webpack.config.dev.js#L149).

This allowed the build to progress a bit further, but now something else is tripping up on `shared/components/Profile.js`, not recognizing it as a jsx file. Here's the error:

    ../shared/components/Profile.js
    Syntax error: Unexpected token (5:16)
    
      3 | class Profile extends React.Component {
      4 |     render() {
    > 5 |         return (<div>Profile</div>);
        |                 ^
      6 |     }
      7 | }
      8 |
