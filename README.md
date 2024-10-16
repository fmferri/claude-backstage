# [Backstage](https://backstage.io)

This is your newly scaffolded Backstage App, Good Luck!

To start the app, run:

```sh
yarn install
yarn dev
```


# [Backstage] GitHub Configuration for Authentication

Make sure that the following has been added to the app-configuration.yaml

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
