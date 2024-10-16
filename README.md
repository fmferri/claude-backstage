# [Backstage](https://backstage.io)

This is your newly scaffolded Backstage App, Good Luck!

To start the app, run:

```sh
yarn install
yarn dev
```

# [Backstage] START DATABASE
You first need to configure a postgres DB.I have used the dockerdesktop with the following command:
docker run --name postgres-back-cat -p 5432:5432 -e POSTGRES_PASSWORD=secret -d postgres

Then you need to add at the app-config.yaml the following:

```sh
backend:
  database:
    client: pg
    connection:
      host: localhost       
      # Default postgresql port, change accordingly to your case
      port: 5432
      user: postgres
      # The password you set earlier
      password: secret
```

```sh
NB: It is really importante that the "database:" word if after within the "backend:" word.
END DATABASE
```

# [Backstage] START AUTHENTICATION
Per impostare Github LOGIN

Go to https://github.com/settings/applications/new to create your OAuth App.
"Homepage URL" = "http://localhost:3000"
"Authorization callback URL" = "http://localhost:7007/api/auth/github/handler/frame".

app-config.yaml
aggiungere questa parte:
```sh
auth:
  # see https://backstage.io/docs/auth/ to learn about auth providers
  environment: development
  providers:
    # See https://backstage.io/docs/auth/guest/provider
    guest: {}
    github:
      development:
        clientId: 'Ov23liB0338MK9lf6rrV'
        clientSecret: '64e62015b80946c1cfcff30661f46d85c926dcf2'
        signIn:
          resolvers:
            # Matches the GitHub username with the Backstage user entity name.
            # See https://backstage.io/docs/auth/github/provider#resolvers for more resolvers.
            - resolver: usernameMatchingUserEntityName
```

LATO FRONTEND

Open packages/app/src/App.tsx and below the last import line, add:

import { SignInProviderConfig } from '@backstage/core-components';

const githubProvider: SignInProviderConfig = {
  id: 'github-auth-provider',
  title:'GitHub',
  message:'Sign in using GitHub',
  apiRef: githubAuthApiRef,
};

Search for const app = createApp({ in this file, and substitute the following:
    components: {
     SignInPage: props => <SignInPage {...props} auto providers={['guest']} />,
    },
with this:

components: {
  SignInPage: props => (
    <SignInPage
      {...props}
      auto
      provider={githubProvider}
    />
  ),
},

Search for     "SignInPage: props => <SignInPage {...props} auto providers={['guest']} />,"

e aggiungi 'github' like this 

    SignInPage: props => <SignInPage {...props} auto providers={['guest', 'github']} />,

LATO BACKEND

from your Backstage root directory
yarn --cwd packages/backend add @backstage/plugin-auth-backend-module-github-provider

then add this linein packages/backend/src/index.ts

backend.add(import('@backstage/plugin-auth-backend-module-github-provider'));

ADDING A USER

First open the /examples/org.yaml file in your text editor of choice

At the bottom we'll add the following YAML:

```sh
---
apiVersion: backstage.io/v1alpha1
kind: User
metadata:
  name: YOUR GITHUB USERNAME
spec:
  memberOf: [guests]
END AUTHENTICATION
```

# [Backstage] START SET UP A GITHUB INTEGRATION
This is useful if you want to import things form the github reposiotories into the catalog.

Make sure that you have added the rights to import the relvant entities by doing the following:
You need to add the relevant entiy into the app-config.yaml here:

```sh
catalog:
  rules:
    - allow: [Component, System, API, Resource, Location, Group, User, Template]
```

Create your Personal Access Token by opening the "GitHub token creation page (https://github.com/settings/tokens/new)".

In your app-config.local.yaml go ahead and add the following:

app-config.local.yaml
integrations:
  github:
    - host: github.com
      token: ghp_urtokendeinfewinfiwebfweb # this should be the token from GitHub
END SET UP A GITHUB INTEGRATION
