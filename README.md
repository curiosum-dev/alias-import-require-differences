# Alias, import, require and use in Elixir. What are the differences?

This is a repo with a couple of examples meant to be used as a reference for ["What's the difference between alias, import, require and use in Elixir?"](https://curiosum.dev/blog/alias-import-require-use-in-elixir) article published at [curiosum.dev](https://curiosum.dev). You can treat it as a short cheat sheet for the most common dependency handling operations in Elixir.

## Example modules

These modules are going to be used as a use case example for `alias` and `import`:

```elixir
defmodule Curiosum.Authentication.Admin do
 defstruct [:id, :email]

 def changeset(admin, params \\ %{}) do
 # ...
 end
end

defmodule Curiosum.Authentication.User do
 defstruct [:id, :email]

 def changeset(user, params \\ %{}) do
 # ...
 end
end
```

## Alias

You can declare `alias` in the following way:

```elixir
# alias will be named AdminResource
alias Curiosum.Authentication.Admin, as: AdminResource

# alias will be named based on the last part of module name - Admin
alias Curiosum.Authentication.Admin

# aliases for both resources
alias Curiosum.Authentication.Admin
alias Curiosum.Authentication.User

# aliases for both resources shortcut
alias Curiosum.Authentication.{Admin, User}

# aliases with disabled warning (will not be printed if alias is not used)
alias Curiosum.Authentication.{ Admin, User }, warn: false
```

## Import

You can declare `import` in the following way:

```elixir
# imports all functions and macros from module
import Curiosum.Authentication.Admin

# imports only changeset function with arity 2
import Admin, only: [changeset: 2]

# imports only functions
import Admin, only: :functions

# imports only macros
import Admin, only: :macros

# imports all functions and macros with disabled warning
import Admin, warn: false
```

## Require

With given macro as an example:

```elixir
defmodule Conditional do
 defmacro custom_unless(clause, do: expression) do
 quote do
 if(!unquote(clause), do: unquote(expression))
 end
 end
end
```

You can declare `require` in the following way:

```elixir
# makes macros in given module publicly available
require Conditional

# makes macros in given module publicly available and gives optional alias to module
require Conditional, as: Cond
```

## Use

With a given module as an example:

```elixir
defmodule Curiosum.Authentication.Schema do
 defmacro __using__(_opts) do
 quote do
 use Ecto.Schema

 import Ecto
 import Ecto.Changeset
 import Ecto.Query

 def changeset(struct, params \\ %{}) do
 # ...
 end
 end
 end
end
```

You can `use` it in the following way:

```elixir
# injects code declared with __using__ of given module
use Curiosum.Authentication.Schema

# passes optional arguments to __using__
use Curiosum.Authentication.Schema, :test
```
