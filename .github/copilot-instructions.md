# Copilot Instructions for mermaid-ganttoo

## Project Overview

This is a browser-based interactive Gantt chart editor built with Svelte 5 and Vite. It allows users to create and edit Gantt charts with real-time Mermaid code generation, featuring a split-pane interface with table editing on the left and live chart preview on the right.

## Tech Stack

- **Framework**: Svelte 5 (component-based reactive framework)
- **Build Tool**: Vite 7 (fast builds, HMR)
- **Diagram Library**: Mermaid.js 11.12.2 (locally bundled, no CDN)
- **Language**: JavaScript (ES6+)
- **UI**: Chinese language interface (zh-CN)

## Architecture

### Component Structure

```
src/
├── App.svelte              # Main application component - state management, Mermaid rendering
├── lib/
│   ├── Section.svelte      # Section/table component - manages individual sections with tasks
│   └── Notification.svelte # Toast notification component - user feedback
└── main.js                 # Entry point
```

### Key Features

1. **Section Management**: Add/delete sections, editable section names
2. **Task Management**: CRUD operations on tasks within sections
3. **Task Fields**: Name, ID, start date, dependencies, duration
4. **Real-time Code Generation**: Converts table data to Mermaid syntax
5. **Live Preview**: Renders Gantt chart using local Mermaid.js
6. **Modern UX**: Clipboard API with fallback, toast notifications

## Development Workflow

### Commands

```bash
npm install    # Install dependencies
npm run dev    # Development server with HMR (localhost:5173)
npm run build  # Production build to dist/
npm run preview # Preview production build
```

### Build Output

- Production bundle: ~532KB minified, ~154KB gzipped
- All Mermaid diagram types included
- No external runtime dependencies

## Code Style and Conventions

### Svelte Components

- Use `<script>`, `<template>`, `<style>` structure
- Leverage Svelte's reactivity with `$:` and reactive statements
- Use `createEventDispatcher` for component communication
- Keep state management in parent components (App.svelte)

### Naming Conventions

- Components: PascalCase (e.g., `Section.svelte`)
- Variables/functions: camelCase (e.g., `addSection`, `renderGantt`)
- Constants: Use const for all non-reassigned variables
- Event handlers: on[Action] pattern (e.g., `onClick`, `onChange`)

### State Management

- Sections array stored in App.svelte
- Each section has: `id`, `name`, `tasks[]`
- Each task has: `name`, `id`, `startDate`, `dependencies`, `duration`
- Use Svelte's reactive assignments (`sections = sections`) for updates

### Mermaid Integration

- Initialize mermaid in `onMount` lifecycle
- Use `mermaid.render()` for async rendering
- Generate Mermaid code format:
  ```
  gantt
      title 甘特图
      dateFormat YYYY-MM-DD
      section [section-name]
      [task-name] : [task-id], [after dependency | start-date], [duration]d
  ```

## File Organization

### Don't Modify

- `node_modules/` - managed by npm
- `package-lock.json` - managed by npm
- `dist/` - build output
- `.vscode/extensions.json` - VS Code recommendations

### Configuration Files

- `vite.config.js` - Vite configuration (Svelte plugin)
- `svelte.config.js` - Svelte compiler options
- `jsconfig.json` - JavaScript/editor configuration
- `index.html` - HTML entry point

## Common Tasks

### Adding a New Feature

1. Determine if it's component-level or app-level
2. For new components: Create in `src/lib/`
3. For app features: Modify `App.svelte`
4. Use Svelte's event system for component communication
5. Test with `npm run dev`

### Modifying Mermaid Output

- Edit `generateMermaidCode()` function in App.svelte
- Follow Mermaid Gantt syntax specifications
- Test edge cases (no date, dependencies, empty fields)

### Styling Changes

- Use scoped `<style>` blocks in components
- Global styles in `:global()` selectors
- Maintain Chinese language UI text
- Follow existing color scheme (green primary, red danger, blue secondary)

## Testing and Validation

### Manual Testing

1. Start dev server: `npm run dev`
2. Test CRUD operations on sections/tasks
3. Verify Mermaid code generation
4. Check chart rendering
5. Test edge cases (empty data, dependencies, date formats)

### Build Validation

1. Run `npm run build`
2. Check for warnings/errors
3. Verify bundle size (should be ~532KB minified)
4. Test with `npm run preview`

## Important Notes

### Language

- UI is in Chinese (zh-CN)
- Comments can be in English or Chinese
- User-facing text must be in Chinese

### Dependencies

- Mermaid is a production dependency (bundled)
- Svelte and Vite are dev dependencies
- Keep dependencies minimal and up-to-date

### Browser Support

- Modern browsers with ES6+ support
- Uses Clipboard API with document.execCommand fallback
- No IE11 support needed

### Security

- No sensitive data handling
- Client-side only application
- No backend/API calls
- All code runs in browser sandbox

## Error Handling

### Mermaid Rendering Errors

- Wrap `mermaid.render()` in try-catch
- Display error message in preview pane
- Provide user guidance on fixing invalid syntax

### User Input Validation

- Allow flexible input (partial data is okay)
- Only generate Mermaid code for complete tasks (name + id)
- Handle empty dependencies/dates gracefully

## Performance Considerations

- Large bundle due to Mermaid (expected, acceptable)
- Use Svelte's reactivity efficiently (avoid unnecessary re-renders)
- Debounce frequent updates if needed
- Keep task list size reasonable (<100 tasks per section)

## Future Enhancements (Ideas)

- Data persistence (localStorage, export/import)
- More Mermaid diagram types
- Drag-and-drop task reordering
- Task templates
- Color coding/status indicators
- Print/export functionality

## Getting Help

- Svelte docs: https://svelte.dev/docs
- Mermaid docs: https://mermaid.js.org/
- Vite docs: https://vitejs.dev/

## Contribution Guidelines

- Maintain existing code style
- Test all changes locally before committing
- Keep commits focused and descriptive
- Update README if adding new features
- Ensure build passes without warnings
