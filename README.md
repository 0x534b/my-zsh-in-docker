# Oh My Zsh Installer for Docker

This is a script to automate [Oh My Zsh](https://ohmyz.sh/) installation in development containers.
Works with any images based on Alpine, Ubuntu, Debian or CentOS.

The goal is to simplify installing zsh in a Docker image for use with [VSCode's Remote Conteiners
extension](https://code.visualstudio.com/docs/remote/containers)

## Usage

Add the following lines in your Dockerfile:

```Dockerfile
RUN sh -c "$(wget -O- https://raw.githubusercontent.com/deluan/zsh-in-docker/master/zsh-in-docker.sh)" -- -t <theme> -p <plugin>
```

Optional arguments:

- `-t <theme>` - Selects the theme to be used. Options are available
  [here](https://github.com/robbyrussell/oh-my-zsh/wiki/Themes). By default the script installs
  and uses [Powerlevel10k](https://github.com/romkatv/powerlevel10k), one the
  "fastest and most awesome" theme for `zsh`. This is the recomended theme, as it is extremely fast
  for git info updates
- `-p <plugin>` - Specifies a plugin to be configured in the generated `.zshrc`. Bundled plugins
  are available [here](https://github.com/robbyrussell/oh-my-zsh/tree/master/plugins).
  If `<plugin>` is a url, the script will try to install the plugin using `git clone`.

Examples:

```Dockerfile
# Default powerline10k theme, no plugins installed
RUN sh -c "$(wget -O- https://raw.githubusercontent.com/deluan/zsh-in-docker/master/zsh-in-docker.sh)"
```

```Dockerfile
# Uses "agnoster" theme, no plugins
RUN sh -c "$(wget -O- https://raw.githubusercontent.com/deluan/zsh-in-docker/master/zsh-in-docker.sh)" -- \
    -t agnoster
```

```Dockerfile
# Uses "git" and "ssh-agent" bundled plugins
RUN sh -c "$(wget -O- https://raw.githubusercontent.com/deluan/zsh-in-docker/master/zsh-in-docker.sh)" -- \
    -p git -p ssh-agent
```

```Dockerfile
# Uses "robbyrussell" theme (original Oh My Zsh theme), uses some bundled plugins and install some more from github
RUN sh -c "$(wget -O- https://raw.githubusercontent.com/deluan/zsh-in-docker/master/zsh-in-docker.sh)" -- \
    -t robbyrussell \
    -p git \
    -p ssh-agent \
    -p https://github.com/zsh-users/zsh-autosuggestions \
    -p https://github.com/zsh-users/zsh-completions
```

## Notes

- As a side-effect, this script also installs `git` and `curl` in your image, if they are not
  available
- By default this script install the `powerlevel10k` theme, as it is one of the fastest and most
  customizable themes available for zsh. If you want the default Oh My Zsh theme, uses the option
  `-t robbyrussell`
