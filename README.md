# Glypha

**Make studio-grade diagrams programmatically with React & AI Agents.**

Glypha is an open-source, code-first diagramming engine built for developers and AI coding agents. Write diagrams in clean React/TSX, refine layouts visually in a local interactive studio, and let agents like Claude Code generate non-overlapping system architectures, workflows, sequence timelines, and database schemas straight from your codebase.

---

## Why Glypha?

AI models like Claude are great at understanding codebases, but terrible at spatial math—resulting in messy, overlapping Mermaid or PlantUML diagrams. Glypha fixes this by separating **structure**, **layout**, and **design**:

- **React / TSX as the DSL:** Compose reusable `<Node>`, `<Group>`, `<Lane>`, `<SchemaTable>`, and `<Edge>` components with Tailwind CSS instead of brittle text syntax.
- **Zero-Overlap Guarantee:** Built-in spatial bounding-box linting (`glypha check`) catches overlapping nodes or clipped labels so AI agents automatically self-correct before rendering.
- **Universal Diagram Engine:** Built for **any** diagram type—system architecture, C4 models, business workflows, swimlanes, sequence diagrams, and relational ERDs.
- **Local Interactive Studio:** Preview diagrams live with Hot Module Replacement (HMR), switch themes, toggle animated flows, and drag nodes to sync coordinates back to your `.tsx` code.
- **Headless CLI & CI/CD Exports:** Render crisp **SVG**, **4K PNG**, **PDF**, or **standalone interactive HTML** files using Playwright.
- **Native MCP & Agent Skills:** Ships with an official MCP server and `SKILL.md` so Claude Code, Cursor, and Windsurf produce studio-quality diagrams on the first try.

---

## Quick Start

Scaffold a new Glypha project in seconds:

```bash
npm create glypha@latest
cd my-diagrams
npx glypha studio
```

---

## Example: Writing a Diagram in TSX

Create a `system.diagram.tsx` file using Glypha's declarative primitives:

```tsx
import { Diagram, Group, Node, Edge } from '@glypha/core';

export default function PaymentArchitecture() {
  return (
    <Diagram direction="LR" theme="editorial">
      <Node id="client" title="Web & Mobile App" icon="smartphone" variant="external" />

      <Group id="cloud" title="Production Cluster">
        <Node id="gateway" title="API Gateway" subtitle="Rate Limit & Auth" icon="shield" variant="primary" />
        <Node id="worker" title="Payment Service" subtitle="Node.js / TypeScript" icon="cpu" />
        <Node id="db" title="Ledger DB" subtitle="PostgreSQL" icon="database" variant="storage" />
      </Group>

      <Edge from="client" to="gateway" label="HTTPS / JSON" animated />
      <Edge from="gateway" to="worker" label="gRPC" />
      <Edge from="worker" to="db" label="ACID Tx" />
    </Diagram>
  );
}
```

---

## Supported Diagram Paradigms

| Diagram Type | Core Primitives | Ideal Use Cases |
| :--- | :--- | :--- |
| **System Architecture** | `<Diagram>`, `<Group>`, `<Node>` | Cloud infra, microservices, C4 models, network topologies |
| **Workflows & Processes** | `<Diagram>`, `<Lane>`, `<StepNode>` | Business logic, user journeys, CI/CD pipelines, BPMN |
| **Sequence & Timelines** | `<SequenceDiagram>`, `<Actor>`, `<Message>` | API request lifecycles, event-driven flows, auth handshakes |
| **Data & Schemas** | `<SchemaTable>`, `<PortEdge>` | Database ERDs, UML class diagrams, event payload contracts |

---

## CLI Commands

```bash
# Launch the local visual studio with live HMR
npx glypha studio

# Validate layout for overlapping boxes or truncated labels (used by AI agents)
npx glypha check system.diagram.tsx

# Export to high-resolution PNG, SVG, PDF, or standalone interactive HTML
npx glypha render system.diagram.tsx --format png --scale 3
npx glypha render system.diagram.tsx --format html --out ./docs/arch.html
```

---

## Using Glypha with Claude Code (MCP & Skills)

Glypha includes a built-in MCP server so your AI coding agent can inspect your repository, generate `.diagram.tsx` files, and verify zero layout collisions automatically.

1. **Add the MCP server** to your Claude Code or Claude Desktop configuration:
   ```json
   {
     "mcpServers": {
       "glypha": {
         "command": "npx",
         "args": ["-y", "@glypha/mcp"]
       }
     }
   }
   ```
2. **Prompt Claude:**
   > *"Scan this repository and generate an interactive Glypha architecture diagram showing how authentication and background jobs interact. Run `glypha check` to verify there are no overlapping nodes."*

---

## Roadmap

- [x] Project architecture & core TSX component spec
- [ ] `@glypha/core` — React Flow + ELKjs compound layout engine
- [ ] `@glypha/studio` — Local Vite studio with drag-to-code AST sync
- [ ] `@glypha/cli` — Headless Playwright exporter & bounding-box linter
- [ ] `@glypha/mcp` — Official MCP server & `SKILL.md` for Claude Code / Cursor
- [ ] Community theme gallery (`editorial`, `blueprint`, `midnight`, `sketch`)

---

## Contributing

Contributions are welcome! Whether you want to build custom themes, add domain icon packs, or improve layout algorithms:

1. Fork the repository
2. Create your feature branch (`git checkout -b feat/amazing-theme`)
3. Commit your changes (`git commit -m 'Add sketch theme'`)
4. Push to the branch (`git push origin feat/amazing-theme`)
5. Open a Pull Request

---

## License

MIT © [Glypha Contributors](https://github.com)
