# Template repository for gptcli plugin

This is a template repository for building a gptcli plugin.

## Usage

Just write your plugin in the `yml` file and run it with `gptcli`.

For example,  I wrote a joke plugin in `joke.yml` which can be tell you a joke.

And I can test it with `gptcli`:

```bash
gptcli joke.yml
```

## Install
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

## Development

### grammar


## License

[MIT](LICENSE)


