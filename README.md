# Kryo Language Support for Windsurf/Devin

Complete language support for the Kryo systems programming language in Windsurf/Devin IDE with advanced features optimized for AI-assisted development.

## Features

### Core Language Support
- **Syntax Highlighting**: Enhanced syntax highlighting for all Kryo constructs
- **100+ Code Snippets**: Comprehensive snippets for common Kryo patterns
- **Language Configuration**: Advanced auto-closing, bracket matching, and indentation
- **File Icon**: Custom icon for `.kyo` files with theme support

### Advanced IDE Features
- **IntelliSense**: Context-aware code completion for memory domains, capacity types, and resource types
- **Diagnostics**: Real-time linting with smart error detection
- **Code Formatting**: Multi-style formatting (standard, compact, expanded)
- **Inlay Hints**: Type inference hints and parameter information
- **Code Actions**: Quick fixes for common issues
- **Go to Definition**: Navigation support (future LSP integration)
- **Semantic Highlighting**: Enhanced semantic token support

### Build Integration
- **Build Commands**: Build, run, check, format, and lint commands
- **Terminal Integration**: Dedicated terminal for Kryo operations
- **Problem Matching**: Automatic error parsing and display in Problems panel
- **Keyboard Shortcuts**: Optimized shortcuts for common operations

### Windsurf/Devin Specific Features
- **AI Context Awareness**: Enhanced context for AI code generation
- **Smart Snippets**: Snippets optimized for AI-assisted development
- **Documentation Integration**: Quick access to Kryo documentation
- **Status Bar Integration**: Status bar item for quick access
- **Theme Support**: Custom colors for memory domains and types

## Installation

### From Windsurf/Devin Marketplace (Coming Soon)

1. Open Windsurf/Devin
2. Go to Extensions (Ctrl+Shift+X)
3. Search for "Kryo Language Support for Windsurf"
4. Click Install

### Manual Installation

1. Clone this repository
2. Open Windsurf/Devin
3. Go to Extensions → Install from VSIX
4. Select the `.vsix` file (build it first with `vsce package`)

## Configuration

The extension provides extensive configuration options:

### Compiler Settings

```json
{
  "kryo.compilerPath": "kryo",
  "kryo.linting.enabled": true,
  "kryo.formatting.enabled": true,
  "kryo.formatting.style": "standard",
  "kryo.maxProblems": 100,
  "kryo.trace.server": "off"
}
```

### Advanced Settings

```json
{
  "kryo.autocomplete.enabled": true,
  "kryo.semanticHighlighting.enabled": true,
  "kryo.inlayHints.enabled": true
}
```

### Settings Reference

- `kryo.compilerPath`: Path to the Kryo compiler executable (default: `kryo`)
- `kryo.linting.enabled`: Enable/disable linting (default: `true`)
- `kryo.formatting.enabled`: Enable/disable code formatting (default: `true`)
- `kryo.formatting.style`: Formatting style - `standard`, `compact`, or `expanded` (default: `standard`)
- `kryo.maxProblems`: Maximum number of problems to report (default: `100`)
- `kryo.trace.server`: LSP server trace level - `off`, `messages`, or `verbose` (default: `off`)
- `kryo.autocomplete.enabled`: Enable intelligent autocomplete (default: `true`)
- `kryo.semanticHighlighting.enabled`: Enable semantic highlighting (default: `true`)
- `kryo.inlayHints.enabled`: Enable inlay hints for type inference (default: `true`)

## Commands

The extension provides the following commands:

### Build Commands
- `Kryo: Build Current File` (Ctrl+Shift+B): Build the current Kryo file
- `Kryo: Run Current File` (F5): Build and run the current Kryo file
- `Kryo: Type Check Current File` (Ctrl+Shift+T): Type check without building

### Code Quality Commands
- `Kryo: Format Current File` (Shift+Alt+F): Format the current file
- `Kryo: Lint Current File`: Run linter on the current file

### Server Commands
- `Kryo: Restart Language Server`: Restart the language server if needed

### Documentation Commands
- `Kryo: Show Documentation`: Open Kryo documentation in browser

## Code Snippets

The extension includes 100+ code snippets organized by category:

### Basic Patterns
- `main` - Create a main function
- `fn` - Create a function
- `let` - Create a let statement
- `let_mut` - Create a mutable let statement
- `if` - Create an if statement
- `while` - Create a while loop
- `for` - Create a for loop
- `match` - Create a match statement

### Memory Domain Patterns
- `stack` - Create a stack-allocated variable
- `heap` - Create a heap-allocated variable
- `persistent` - Create a persistent variable
- `gpu` - Create a GPU-allocated variable
- `shared` - Create a shared memory variable
- `distributed` - Create a distributed memory variable

### Type Patterns
- `array` - Create a fixed-size array
- `bounded_vec` - Create a bounded vector
- `bounded` - Create a bounded integer
- `resource` - Create a resource

### Advanced Patterns
- `struct` - Create a struct
- `enum` - Create an enum
- `trait` - Create a trait
- `impl` - Create an impl block
- `unsafe` - Create an unsafe block
- `contract` - Create a function with contracts

### Smart Types
- `box` - Create a Box type
- `rc` - Create an Rc type
- `arc` - Create an Arc type
- `slice` - Create a slice type
- `lifetime` - Create a lifetime annotation

## AI-Assisted Development

The extension is optimized for Windsurf/Devin's AI features:

### Context Awareness
- Enhanced syntax highlighting provides better context for AI
- Smart snippets help AI generate idiomatic Kryo code
- Memory domain awareness helps AI suggest appropriate memory management

### Code Generation
- Snippets are designed to work well with AI code completion
- Type hints and inlay hints improve AI understanding
- Documentation integration helps AI provide accurate suggestions

### Error Recovery
- Smart diagnostics help AI understand and fix errors
- Code actions provide quick fixes that AI can suggest
- Linting rules guide AI toward best practices

## Keyboard Shortcuts

- `Ctrl+Shift+B`: Build current file
- `F5`: Run current file
- `Ctrl+Shift+T`: Type check current file
- `Shift+Alt+F`: Format current file

## Color Themes

The extension defines custom colors for different memory domains:

- **Stack Memory**: Green (stack allocation)
- **Heap Memory**: Blue (heap allocation)
- **Persistent Memory**: Orange (persistent storage)
- **GPU Memory**: Purple (GPU memory)

These colors can be customized in your theme settings.

## Development

### Building the Extension

```bash
npm install
npm run compile
```

### Packaging the Extension

```bash
npm install -g vsce
vsce package
```

### Running in Development Mode

1. Press `F5` in Windsurf/Devin
2. A new Windsurf/Devin window will open with the extension loaded

### Testing

The extension includes built-in testing capabilities:

```bash
npm test
```

## Requirements

- Windsurf/Devin IDE or VS Code 1.80.0 or higher
- Kryo compiler (for build/run functionality)
- Node.js 18+ (for development)

## Troubleshooting

### Language Server Issues
If the language server fails to start:
1. Run `Kryo: Restart Language Server` command
2. Check the "Kryo Language Server" output channel
3. Verify the compiler path in settings

### Formatting Issues
If formatting doesn't work as expected:
1. Check the formatting style setting
2. Try different formatting styles (standard, compact, expanded)
3. Ensure the file is saved before formatting

### Linting Issues
If linting shows unexpected errors:
1. Check that linting is enabled in settings
2. Verify the Kryo compiler is installed
3. Check the Problems panel for detailed error messages

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

### Development Guidelines
- Follow the existing code style
- Add tests for new features
- Update documentation for changes
- Test in both VS Code and Windsurf/Devin

## License

MIT License - see LICENSE file for details

## Support

For issues and feature requests, please use the [GitHub Issues](https://github.com/kryo-lang/kryo-windsurf/issues) page.

## Kryo Language

For more information about the Kryo language, visit:

- [Kryo Language Specification](https://github.com/kryo-lang/kryo)
- [Kryo Documentation](https://docs.kryo-lang.org)
- [Kryo Tutorial](https://docs.kryo-lang.org/tutorial)
- [Kryo Best Practices](https://docs.kryo-lang.org/best-practices)

## Acknowledgments

This extension is inspired by:
- Rust Analyzer for advanced language features
- Zig Language Support for simplicity
- C/C++ extensions for systems programming features
- Windsurf/Devin for AI-assisted development paradigms

## Roadmap

### Future Features
- Full LSP server integration
- Advanced semantic analysis
- Go to definition and references
- Code refactoring support
- Integrated testing support
- Debugging support
- Performance profiling
- Memory visualization
- GPU kernel debugging
- Distributed memory debugging

### AI Enhancements
- AI-powered code generation
- Smart refactoring suggestions
- Automated error fixing
- Performance optimization suggestions
- Memory usage analysis
- Best practices enforcement