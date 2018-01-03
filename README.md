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
