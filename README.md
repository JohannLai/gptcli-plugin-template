# 🛠️ Template repository for gptcli plugin

This is a template repository for building a gptcli plugin.

## 🤖 How to use ?

Just write your plugin in the `yml` file and run it with `gptcli`.

For example,  I wrote a joke plugin in `joke.yml` which can be tell you a joke.

And I can test it with `gptcli`:

```bash
gptcli joke.yml
```

## 📦 Install

when you finish your plugin, you can install it with `gptcli`, with your github username and repository name.

```bash
# username/repository
# for example, my plugin is in
gptcli install johannlai/joke
```

if you don't want to publish your plugin, you can install it with local file.

```bash
mv joke.yml ~/.config/gptcli/plugins/joke/joke.yml
```

## 📃 Documentation
The `YAML` file configures description.

### name
the plugin name, it will be used in `gptcli install` command.


```yml
name: Plugin Name
```

### description
the plugin description

```yml
description: Plugin Description
```

### author
the plugin author

```yml
author: Plugin Author
```

### help
the plugin help message

```yml
help: |
  Plugin help information

  Usage:
  ❯ [Plugin Name] [Plugin Arguments]

  Description:
  [Plugin Description]
```

### env
the default environment variables

```yml
env:
  [Environment Variable Name]: [Environment Variable Value]

### steps

the plugin steps

```yml
steps:
  - name: Step Name
    uses: Command or script
    with:
      Parameter 1: Value 1
      Parameter 2: Value 2
    export:
      # export the output key to environment variable
      # see more output key below
      [Output Key]: [Env variable Name]
  - name: Step Name
    script: |
      Script
```

### steps.name
the step name

### steps.uses
the builtin job to run

now, we have below builtin jobs:

#### `script`
run a shell script

```yml
steps:
  - name: Step Name
    script: |
       echo "Hello World"
```

**output key:**

use `::set-output name=[Output Key]::[Output Value]` to set output key

#### `confirm`

confirm with user, you can see more in [commit plugin](https://github.com/JohannLai/gptcli/blob/main/src/plugins/commit.yml)

**output key:**
| Output Key | Description     | type    |
| ---------- | --------------- | ------- |
| `answer`   | the user answer | boolean |

```yml
  - name: "ask if user want to execute the command"
    uses: "gpt:confirm"
    with:
      message: "Would you like to use this commit message ⬆️ ? "
      default: true
    export:
      answer: ANSWER
  - name: "execute the command"
    if: $ANSWER == true
    script: |
      echo "execute the command"
```

#### `createChatCompletion`
ask ChatGPT to get a answer, you can see more in [translate plugin](https://github.com/JohannLai/gptcli/blob/main/src/plugins/translate.yml)


**input key:**
use `with` to set input key

| Input Key          | Description                       | type   |
| ------------------ | --------------------------------- | ------ |
| `messages`         | the messages to send to ChatGPT   | array  |
| `messages.role`    | the message role, `user` or `bot` | string |
| `messages.content` | the message content               | string |


**output key:**
| Output Key         | Description         | type   |
| ------------------ | ------------------- | ------ |
| `response_content` | the answer response | string |

```yml
  - name: "ask ai to translate"
    uses: "gpt:createChatCompletion"
    with:
      messages:
        - role: "user"
          content: "You are a translator. Translate a piece of text into $LANG without explanation. \n the origin text is $params_0"
    export:
      response_content: RESULT
```

### export
export the output key to environment variable

export the left value to the right value. The left value is from the job output. in `script`, you can use `::set-output name=[Output Key]::[Output Value]` to set output key.

In builtin job, you can see more in the builtin job section. They will output some key, and you can use them in `export` section.


```yml
export:
  [Output Key]: [Env variable Name]
```


### if
the condition to run the step. If the condition is false, the step will be skipped.

`Condition` is a js expression, you can use the environment variable in `env` section.

**input key:**
| Input Key   | Description                   | type          |
| ----------- | ----------------------------- | ------------- |
| `condition` | the condition to run the step | js expression |


```yml
if: "100 > 10 "
```

## 📃 License

[MIT](LICENSE)


