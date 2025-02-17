## First, what is [mise](https://github.com/jdx/mise)?

Straight to the point, **mise-en-place** (or simply **mise**) is a modern **dev-tool manager** that simplifies managing multiple versions of programming languages and development tools. It serves a similar purpose as **asdf**, but with a focus on performance, ease of use, and broader functionality.

Just like:

- **asdf** manages versions of development tools,
- **direnv** manages environment variables dynamically,
- **make** automates development tasks,

**mise** combines version management with a seamless experience for developers.

### Why should I use mise instead of ASDF (or others)?

While **asdf** is a well-known tool, **mise** offers some advantages that might make it a better fit for your workflow:

1. **All-in-One Solution:** mise provides a unified experience, reducing the need for additional plugins and manual setup.
2. **Better Performance:** It’s optimized for speed, making tool switching and environment loading faster.
3. **Compatibility:** It supports `.tool-versions` files (used by asdf), making migration easy.
4. **Simplified Configuration:** Unlike asdf, mise is designed with sane defaults and minimal configuration hassle.
5. **Declarative & Reproducible Environments:** Define tools per project with a single file, making onboarding and collaboration smoother.

### How does mise work?

**mise** works by reading configuration files (like `.tool-versions` or `.mise.toml`) and ensuring that the correct versions of tools are available in your shell environment. It integrates seamlessly with popular shells like **bash, zsh, and fish**, making it easy to use across different systems.

### Installation & First Steps

To install **mise**, simply run:

```bash
curl -fsSL https://mise.run/install.sh | sh #On linux OS
```
Or, if you are using Homebrew
```bash
brew install mise 
```

After installation, you can add it to your shell profile:

```
eval "$(mise activate zsh)" #Replace `zsh` with your shell if needed
```
And last, activate:

```bash
mise activate
```

Check if everything is right:

```bash
mise doctor
```


Install a tool (e.g., Elixir, because Elixir is great):

```bash
mise use --global elixir@latest erlang@latest
```

Check the installed tools:

```bash
mise ls
```

And you might have something like that:

```bash
Tool    Version        Source                      Requested
elixir  1.18.2-otp-27  ~/.config/mise/config.toml  latest
erlang  27.2.2         ~/.config/mise/config.toml  latest
node    22.14.0        ~/.config/mise/config.toml  22
```

