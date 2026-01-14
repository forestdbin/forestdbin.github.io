# Setup

[hexo docs, Getting Started, Overview](https://hexo.io/docs/)

## Node.js

[Download Node.js](https://nodejs.org/en/download)

```bash
# Download and install nvm:
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash

# in lieu of restarting the shell
\. "$HOME/.nvm/nvm.sh"

# Download and install Node.js:
nvm install 22

# Verify the Node.js version:
node -v # Should print "v22.16.0".
nvm current # Should print "v22.16.0".

# Verify npm version:
npm -v # Should print "10.9.2".
```

## Hexo

```bash
$ git clone https://github.com/forestdbin/forestdbin.github.io.git
$ npm install -g hexo-cli
$ npm install
$ hexo server
```

```bash
hexo new foobar -s foobar # -p dir/foobar.md
```
