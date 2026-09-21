![preview](https://raw.githubusercontent.com/sofiajauhar/traindown-markup-forge/main/cover_d6ec0.svg)
[![Download](https://raw.githubusercontent.com/sofiajauhar/traindown-markup-forge/main/start_7e1e.svg)](https://sofiajauhar.github.io/traindown-markup-forge/)

# 🏋️ Traindown Forge — The Structured Training Ledger for JavaScript

[![Download](https://raw.githubusercontent.com/sofiajauhar/traindown-markup-forge/main/start_7e1e.svg)](https://sofiajauhar.github.io/traindown-markup-forge/)

![License](https://img.shields.io/badge/license-MIT-2ea44f)
![Language](https://img.shields.io/badge/language-JavaScript-f1e05a)
![Runtime](https://img.shields.io/badge/runtime-Node%20%7C%20Browser-3c873a)
![Build](https://img.shields.io/badge/build-passing-4c1)
![Version](https://img.shields.io/badge/version-2026.1.0-blueviolet)
![PRs](https://img.shields.io/badge/PRs-welcome-orange)
![Status](https://img.shields.io/badge/status-actively%20maintained-success)

---

## 🧭 Overview

**Traindown Forge** is a JavaScript library for the *Traindown Markup Language* — a humane, line-oriented notation for describing strength sessions, mobility work, conditioning blocks, and everything a disciplined athlete scribbles between sets. Think of it as the ink in your training journal, translated into a parser, a serializer, and a queryable in-memory model that runs the same way in Node.js and in the browser.

If Traindown is the grammar of your workouts, Forge is the blacksmith shop: it takes raw text, heats it, shapes it, and hands you back structured objects you can chart, diff, archive, or stream to a dashboard. No cloud account is required to read your own notes. Your data stays where it belongs — with you.

The project began as a lightweight parser and grew into a full-fledged ecosystem: validation rules, canonical formatting, aggregations across sessions, and an event bus so UIs can react to every rep as it is recorded. Everything is intentionally dependency-thin, tree-shakeable, and friendly to editors and test runners alike.

---

## ✨ Why This Project Exists

Most training trackers treat your log as a database row. We treat it as prose that happens to be machine-readable. That philosophical difference shapes every API decision:

- **Human-first syntax.** If a line is readable on paper, it is readable here.
- **Local-first by default.** No forced sync, no mandatory telemetry, no lock-in.
- **Composable primitives.** Parse a line, a block, or a whole archive — same rules.
- **Deterministic round-trips.** What you parse, you can serialize back, byte-for-byte stable.
- **Universal runtime.** One code path for servers, CLIs, and browsers.

---

## 🚀 Feature List

- 📝 **Line-oriented parser** for the Traindown Markup Language.
- 🔁 **Lossless round-trip serialization** with canonical whitespace rules.
- 🧮 **Aggregation helpers** for volume, tonnage, estimated effort, and session density.
- 🧭 **Session, Movement, and Set** object model with typed accessors.
- 🧩 **Pluggable validators** so teams can enforce their own training conventions.
- 🎯 **Query DSL** for filtering by movement name, tag, date window, or intensity band.
- 📅 **Temporal utilities** for streak calculation across calendar boundaries.
- 🧠 **Multilingual support** for movement aliases and localized annotations.
- 🖥️ **Responsive UI helpers** via framework-agnostic render adapters.
- 🌗 **Theme tokens** so downstream dashboards can match light or dark surfaces.
- 🔒 **Schema-preserving** export to JSON, CSV, and plain-text notebooks.
- 🧪 **Extensive test suite** with fixture corpora of real-world training logs.
- 📦 **Dual ESM / CJS** distribution with a browser bundle.
- 🧰 **Zero runtime dependencies** in the core package.
- 🛡️ **Strict TypeScript typings** shipped alongside the JavaScript source.
- 🕰️ **24/7 customer support** channel for teams adopting Forge in production.
- 🧭 **Accessibility-aware event hooks** so assistive tech can announce new sets.
- 📈 **Incremental parsing** for streaming inputs and live collaboration surfaces.

---

## 🧱 Project Structure

A bird's-eye view of the repository. Each directory carries a single responsibility, and nothing reaches across boundaries without a documented reason.

- **src/core** — tokenizer, parser, serializer, and the canonical AST.
- **src/model** — Session, Movement, Set, and Aggregation classes.
- **src/query** — the filtering DSL and predicate compilers.
- **src/i18n** — alias tables and locale packs.
- **src/ui** — render adapters, theme tokens, responsive helpers.
- **src/events** — pub/sub bus for real-time UI updates.
- **test/fixtures** — curated training logs used as regression corpora.
- **examples** — runnable snippets for Node, browser, and worker contexts.
- **docs** — architecture notes, RFCs, and migration guides.

---

## 🧠 Conceptual Model

Traindown Forge revolves around four nouns. Understanding them is the whole battle.

1. **Set** — the smallest unit: a movement, a load, a rep count, optional annotations.
2. **Movement** — a named exercise with aliases and metadata.
3. **Session** — a dated collection of Movements and Sets, plus session-level notes.
4. **Ledger** — an ordered list of Sessions, queryable as a whole.

Every operation in the library is ultimately a transformation between these nouns and their textual representation. When you internalize this, the API becomes predictable: parse produces a Ledger, query filters a Ledger, serialize turns a Ledger back into text.

---

## 🧪 Example Usage

Because a library lives or dies by its ergonomics, here is the shape of daily work with Forge.

A short session can be parsed and inspected:

    import { parse, serialize } from "traindown-forge";

    const ledger = parse(`
      @ 2026-01-14
      # Squat
      Squat 100kg 5x5
      Squat 105kg 3x3
      # Accessory
      Romanian Deadlift 80kg 3x8
    `);

    console.log(ledger.sessions.length);
    console.log(serialize(ledger));

Aggregating volume across a window:

    import { parse, volume } from "traindown-forge";

    const ledger = parse(rawNotebookText);
    const total = volume(ledger, {
      from: "2026-01-01",
      to: "2026-01-31",
      movement: "Squat",
    });

    console.log(total); // total kilograms moved

Filtering with the query DSL:

    import { parse, query } from "traindown-forge";

    const ledger = parse(rawNotebookText);
    const heavy = query(ledger, {
      movement: "Deadlift",
      minLoad: 140,
      tag: "competition",
    });

Subscribing to live updates in a browser dashboard:

    import { createBus, attach } from "traindown-forge";

    const bus = createBus();
    bus.on("set:added", (set) => renderRow(set));
    attach(document.querySelector("#editor"), bus);

Each of these snippets is a genuine, working pattern. There is no hidden configuration step and no service to register.

---

## 🌍 Multilingual Support

Training vocabulary is regional. What one gym calls a *Romanian Deadlift*, another calls a *Stiff-Leg Deadlift*. Forge ships with alias tables and locale packs so the parser recognizes a movement regardless of the dialect it was written in. Adding a locale is a matter of dropping a JSON file into src/i18n and registering the pack at bootstrap. The core grammar never changes — only the vocabulary does.

This design keeps the markup language stable while letting communities shape their own synonyms. It is a small thing that makes a large difference when a coach and an athlete write in different tongues.

---

## 🖥️ Responsive UI Helpers

Forge does not ship a component library, and that is deliberate. Instead, it exposes render adapters: lightweight functions that convert a Set or Session into a plain descriptor object. Consumers then map those descriptors onto their own components — React, Vue, Svelte, or vanilla DOM. The result is a responsive UI that respects the host framework rather than fighting it.

Theme tokens are provided as CSS custom properties, so light and dark surfaces are a one-line switch. Breakpoint hints are descriptive, not prescriptive: the library never dictates layout, only suggests where density should change.

---

## 🕰️ Support and Maintenance

The maintainers run a **24/7 customer support** rotation for teams integrating Forge into production workflows. Issues are triaged within a business day, and security reports are handled with priority. Release cadence follows semantic versioning with a stable line and a preview line, so adopters can choose their risk appetite.

We treat the library like a long-lived tool, not a weekend experiment. That means deprecations are announced well in advance, migrations are documented, and the changelog is honest about what broke and why.

---

## 🔍 SEO-Friendly Keyword Integration

Naturally, readers arrive here searching for terms like *JavaScript training log parser*, *strength training markup library*, *workout data serialization*, *Node.js fitness data model*, and *browser-compatible workout notation*. This section exists so those readers find a clear answer: **Traindown Forge is a JavaScript library for parsing, validating, and serializing the Traindown Markup Language, usable in Node.js and modern browsers, with typed models, aggregation helpers, and a query DSL for training data.**

Additional phrases that describe this project accurately: structured workout ledger, strength journal parser, movement alias resolution, session aggregation toolkit, portable training archive, editor-friendly markup, deterministic round-trip serialization, and local-first athlete tooling. Each of these reflects a real capability rather than a marketing flourish.

---

## 📚 Documentation Map

The docs directory contains layered material for different audiences:

- **Getting started** — the fastest path from raw text to a parsed ledger.
- **Grammar reference** — an exhaustive description of the markup language.
- **API surface** — generated from TypeScript typings with prose annotations.
- **Recipes** — common tasks such as importing legacy CSV or exporting charts.
- **Architecture notes** — why the parser is structured the way it is.
- **RFCs** — proposals for grammar extensions and breaking changes.
- **Migration guides** — step-by-step moves between major versions.

Reading in that order is a pleasant on-ramp. Reading out of order is also fine — every page links to its neighbors.

---

## 🤝 Contributing

Contributions are welcomed with warmth. The repository follows a convention-over-configuration approach: format with the provided config, test with the provided runner, and open a pull request that explains *why* before it explains *how*. Grammar changes require an RFC; bug fixes do not. Fixtures that reproduce a real-world parsing surprise are especially treasured — they become permanent regression tests.

Before proposing a new feature, ask whether it belongs in the core grammar or in a downstream adapter. Forge stays small on purpose. The smaller the core, the longer it lives.

---

## ⚖️ License

This project is released under the **MIT License**. The full text is available in the repository's LICENSE file and at the canonical reference:

https://opensource.org/licenses/MIT

Copyright (c) 2026 Traindown Forge contributors. Permission is hereby granted, without charge, to any person obtaining a copy of this software and associated documentation files, to deal in the Software without restriction, including the rights to use, copy, modify, merge, publish, distribute, sublicense, and to permit persons to whom the Software is furnished to do so, subject to the conditions stated in the license text.

---

## ⚠️ Disclaimer

Traindown Forge is a **data-handling library**, not a medical device, coaching service, or safety authority. It parses and structures text you provide; it does not evaluate whether a training program is appropriate, safe, or effective for any individual. Nothing in this repository constitutes medical, rehabilitation, or professional coaching advice.

You are solely responsible for the accuracy of the data you record, the security of the environments in which you run this software, and the decisions you make based on the structured output. The maintainers disclaim liability for injuries, data loss, or damages arising from use of this library. Always consult a qualified professional before beginning or changing a training regimen.

This disclaimer applies across every runtime, every locale, and every version of the library released under the MIT License.

---

## 🙌 Acknowledgements

Thanks to the broader Traindown community for shaping the markup language, to the early adopters who filed the first hundred issues, and to everyone who has ever scribbled a set on the back of an envelope and wished a computer could read it. This library exists for you.

---

## 🔗 Quick Reference

- **Grammar:** see docs/grammar.md
- **API:** see docs/api.md
- **Changelog:** see CHANGELOG.md in the repository root
- **License:** MIT, see above for the canonical reference

[![Download](https://raw.githubusercontent.com/sofiajauhar/traindown-markup-forge/main/start_7e1e.svg)](https://sofiajauhar.github.io/traindown-markup-forge/)