# Überauth Feide

[![Build Status](https://travis-ci.org/ueberauth/ueberauth_github.svg?branch=master)](https://travis-ci.org/ueberauth/ueberauth_github)
[![Module Version](https://img.shields.io/hexpm/v/ueberauth_github.svg)](https://hex.pm/packages/ueberauth_github)
[![Hex Docs](https://img.shields.io/badge/hex-docs-lightgreen.svg)](https://hexdocs.pm/ueberauth_github/)
[![Total Download](https://img.shields.io/hexpm/dt/ueberauth_github.svg)](https://hex.pm/packages/ueberauth_github)
[![License](https://img.shields.io/hexpm/l/ueberauth_github.svg)](https://github.com/ueberauth/ueberauth_github/blob/master/LICENSE.md)
[![Last Updated](https://img.shields.io/github/last-commit/ueberauth/ueberauth_github.svg)](https://github.com/ueberauth/ueberauth_github/commits/master)

> GitHub OAuth2 strategy for Überauth.

## Installation

1.  Setup your application at https://kunde.feide.no.

2.  Add `:ueberauth_feide` to your list of dependencies in `mix.exs`:

    ```elixir
    def deps do
      [
        {:ueberauth_feide, "~> 0.8"}
      ]
    end
    ```

3.  Add GitHub to your Überauth configuration:

    ```elixir
    config :ueberauth, Ueberauth,
      providers: [
        github: {Ueberauth.Strategy.Feide, []}
      ]
    ```

4.  Update your provider configuration:

    ```elixir
    config :ueberauth, Ueberauth.Strategy.Github.OAuth,
      client_id: System.get_env("FEIDE_CLIENT_ID"),
      client_secret: System.get_env("FEIDE_CLIENT_SECRET")
      redirect_uri: System.get_env("FEIDE_REDIRECT_URI")
    ```

    Or, to read the client credentials at runtime:

    ```elixir
    config :ueberauth, Ueberauth.Strategy.Github.OAuth,
      client_id: {:system, "FEIDE_CLIENT_ID"},
      client_secret: {:system, "FEIDE_CLIENT_SECRET"}
      redirect_uri: {:system, "FEIDE_REDIRECT_URI"}
    ```
    ```

5.  Include the Überauth plug in your router:

    ```elixir
    defmodule MyApp.Router do
      use MyApp.Web, :router

      pipeline :browser do
        plug Ueberauth
        ...
       end
    end
    ```

6.  Create the request and callback routes if you haven't already:

    ```elixir
    scope "/auth", MyApp do
      pipe_through :browser

      get "/:provider", AuthController, :request
      get "/:provider/callback", AuthController, :callback
    end
    ```

7.  Your controller needs to implement callbacks to deal with `Ueberauth.Auth`
    and `Ueberauth.Failure` responses.

For an example implementation see the [Überauth Example](https://github.com/ueberauth/ueberauth_example) application.

## Calling

Depending on the configured url you can initiate the request through:

    /auth/feide

Or with options:

    /auth/feide?scope=userid,email

Scope can be configured either explicitly as a `scope` query value on the
request path or in your configuration:

```elixir
config :ueberauth, Ueberauth,
  providers: [
    github: {Ueberauth.Strategy.Feide, [default_scope: "userid,email,openid"]}
  ]
```

<!-- It is also possible to disable the sending of the `redirect_uri` to GitHub. -->
<!-- This is particularly useful when your production application sits behind a -->
<!-- proxy that handles SSL connections. In this case, the `redirect_uri` sent by -->
<!-- `Ueberauth` will start with `http` instead of `https`, and if you configured -->
<!-- your GitHub OAuth application's callback URL to use HTTPS, GitHub will throw an -->
<!-- `uri_mismatch` error. -->

<!-- To prevent `Ueberauth` from sending the `redirect_uri`, you should add the -->
<!-- following to your configuration: -->

<!-- ```elixir -->
<!-- config :ueberauth, Ueberauth, -->
<!--   providers: [ -->
<!--     github: {Ueberauth.Strategy.Github, [send_redirect_uri: false]} -->
<!--   ] -->
<!-- ``` -->

## Copyright and License

Copyright (c) 2015 Daniel Neighman

This library is released under the MIT License. See the [LICENSE.md](./LICENSE.md) file
