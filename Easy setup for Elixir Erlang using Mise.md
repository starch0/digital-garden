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

Or, if you’re using Homebrew (MacOS/Linux):

```

```

After installation, you can add it to your shell profile:

```
eval "$(mise activate zsh)"
```

(replace `zsh` with your shell if needed)

### Example Usage

Install a tool (e.g., Node.js):

```
mise install node@18
```

Set a global version:

```
mise use -g node@18
```

Define a version per project:

```
echo "node 18" > .tool-versions
```

### Conclusion

If you're looking for a **fast, simple, and powerful** alternative to **asdf**, **mise** is worth considering. It brings everything you need into one tool, making development environments easier to manage and more efficient.

Give it a try and see how it fits into your workflow!