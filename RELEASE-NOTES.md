Mapster 0.5.2, built from 29fc14c.

- `Mapster-Windows-Setup.exe`: Windows installer (version 0.4.7)
- `Mapster-Windows.msi`: Windows MSI package (version 0.4.7)
- `Mapster-macOS-AppleSilicon.dmg`: Apple Silicon
- `Mapster-macOS-Intel.dmg`: Intel
- `Mapster-Linux-x86_64.AppImage`: AppImage (version 0.4.7)
- `Mapster-Linux-amd64.deb`: Debian / Ubuntu (version 0.4.7)
- `Mapster-Linux-x86_64.rpm`: Fedora / RHEL / openSUSE (version 0.4.7)
- `mapster-tools-windows-x86_64.zip`: CLI + MCP server — Windows (version 0.4.7)
- `mapster-tools-macos-aarch64.tar.gz`: CLI + MCP server — macOS Apple Silicon
- `mapster-tools-macos-x86_64.tar.gz`: CLI + MCP server — macOS Intel
- `mapster-tools-linux-x86_64.tar.gz`: CLI + MCP server — Linux (version 0.4.7)

Installed copies update themselves: Mapster checks for a newer version when it starts, and Help ▸ Check for Updates checks on demand. Download page: https://skuirrels.github.io/mapster-releases/

## Change log

Mapster was called Mapmeister up to version 0.3.14.

### 0.5.2 · 6 November 2026

- File ▸ Generate XSLT 1.0, 2.0 and 3.0 work again on mappings with large recursive schemas. Generation used to run for minutes without finishing, so the menu items seemed to do nothing; a mapping such as eDC's UniversalShipment now generates in a few seconds.

### 0.5.1 · 5 November 2026

- Drag a field's name in a structure, or a port's name on a function, to start a connection; you no longer have to hit the small handle. A source starts from its output and a target from its input. The handles themselves are also easier to hit.
- The connection settings of a SQL Server or PostgreSQL database show its server and port as fields of their own. Leave the port empty to use 1433 (SQL Server) or 5432 (PostgreSQL); changing either updates the connection string.
- A named SQL Server instance with a port (`server\instance,15433`) connects straight to that port.
- When a database cannot be reached, the message says whether nothing answered (check the address, port and VPN) or nothing was listening on that port.
- The decision table settings are wider, each column is as wide as its contents, and the column names and rule numbers stay in view while you scroll.

### 0.5.0 · 5 November 2026

- REST web service calls. Insert ▸ Web Service Call (REST)… calls an endpoint with Basic, OAuth 2.0 (client credentials, password, refresh token or a token you already have) or custom header authentication. A call can run once for the whole mapping or once for every input it is fed, and its request and response bodies take a JSON or XML Schema you supply, so they show as structures you can map to and from.
- Database mapping for SQL Server, PostgreSQL and SQLite. Insert ▸ Database Tables… reads tables as a source and writes them as a target with insert, update, merge (insert or update) and delete. Each database runs in one transaction that commits only when the whole mapping succeeds.
- Insert ▸ Database Call (Query, Function, Procedure)… runs an SQL query, a function or a stored procedure. Its parameters appear as inputs, and its outputs come from the result it returns: the rows, output parameters, return value and rows affected.
- SQL Server named instances (`server\instance`) connect through SQL Browser.
- Decision tables. Insert ▸ Decision Table… picks the first rule whose conditions all hold and gives its outputs. A condition can be a value, a comparison, a range such as `10..20`, or several alternatives separated by commas, and `-` matches anything.
- Every file component has a file name port. Connect a value to a target's file name to write one file per item, named from that value; a source's file name port gives the path it read.
- MapForce designs now bring in their database components, SELECT statements, stored procedure and function calls, decision tables and dynamic file names. Database passwords are not imported: the connection reads them from an environment variable such as `${SHIPPING_PASSWORD}`. Oracle databases import but cannot connect yet.

### 0.4.7 · 20 October 2026

- FlexText templates can now be made and changed in Mapster. File ▸ New FlexText Template… and Open FlexText Template…, Edit FlexText Template… on a FlexText component, or Edit… in its settings open a designer. Each container shows the part of the sample text it receives as you edit, and the structure the component will have is shown beside it. Saving writes the `.mft` in MapForce's layout and refreshes the components that use it.
- + Structure… now accepts a FlexText template (`.mft`), and the template field in a FlexText component's settings has a button to choose the file.
- Large XML Schemas load much faster. A recursive type is now expanded once instead of twice, and nodes deeper in the recursion open on demand to any depth. The HL7 v3 example schema goes from 2.5 million nodes to 155 thousand.
- Large structures open quickly on the canvas: branches load when you open them, so opening the HL7 v3 example sends 0.6 MB to the window instead of 20 MB. Running, validating, generating XSLT and saving still use the whole structure.
- The rows of the CSV, EDI and Excel settings in a component's Properties use the full width of the dialog again.

### 0.4.6 · 20 October 2026

- Copy-All connections now show what they copy. Every child item under the connected row has a filled handle, and a line standing off the component joins them to that row, as in MapForce, so you no longer have to open the connection's menu to see that it copies child items.

### 0.4.5 · 19 October 2026

- The installers now include the `mapster-cli` command-line tool, so mappings run from a terminal without Rust or Cargo. Choose Help ▸ Install Command-Line Tool… once to put it on your PATH: on macOS it links `/usr/local/bin/mapster-cli`, on Windows it adds Mapster's folder to your Path, and the Linux `.deb` and `.rpm` already install `/usr/bin/mapster-cli`.
- `mapster-cli run --input FILE` reads the source from FILE instead of the input file the mapping names. Use `--input NAME=FILE` when the mapping has several sources.
- `mapster-cli run --out DIR` and `mapster-cli xslt --out DIR` create the folder when it doesn't exist.

### 0.4.4 · 18 October 2026

- Designs with thousands of connections no longer lock the window. Connections to rows that are scrolled out of view or inside a collapsed branch are drawn as one line to the box's edge or to the branch, and hovering it says how many it stands for. ContainerASN.mfd, with 9,756 connections, now draws 49 lines.

### 0.4.3 · 18 October 2026

- Large structure trees stay responsive. A component's tree now draws only the rows in its scroll box, so expanding a branch of thousands of nodes, or opening a design such as one with 10,000 connections, no longer stalls the window.

### 0.4.2 · 18 October 2026

- Opening a mapping while another is open no longer locks the window. Components of the previous mapping with the same ids were drawn once more with the new mapping's collapsed branches, so a large structure tree opened fully for a moment; on macOS that could take minutes.

### 0.4.1 · 18 October 2026

- The macOS downloads open again. In 0.4.0, macOS reported a downloaded Mapster as damaged and offered only to move it to the Bin; the app is now signed as a whole, so it opens like any app from outside the App Store.

### 0.4.0 · 18 October 2026

- **Mapmeister is now Mapster.** The download site moves to https://skuirrels.github.io/mapster-releases/. Copies of Mapmeister don't update to Mapster: uninstall Mapmeister and install Mapster. View settings, recent files and AI settings start fresh.
- The command-line tools are now `mapster-cli` and `mapster-mcp`; update any MCP client configuration that starts `mapmeister-mcp`.
- EDI configurations go in `~/.mapster/edi` or `MAPSTER_EDI_CONFIG`; `~/.mapmeister/edi` and `MAPMEISTER_EDI_CONFIG` still work.
- `.mmap` mapping files are unchanged.

### 0.3.14 · 18 October 2026

- Running a mapping with a large output no longer locks up the window. The output pane, and the text in the AI consent dialog, now show only the lines in view, so macOS accessibility queries stay fast.

### 0.3.13 · 18 October 2026

- **AI assist:** your own Claude Code or Codex CLI changes the mapping and you apply it. Open the AI panel with Ctrl+Shift+A, or use Fix with AI on a problem in Messages, Fix Problems with AI or Explain with AI. Nothing is sent until AI is on, and the panel lists every change before Apply Changes makes it one undoable step.
- The agent works on a scratch copy of the open mapping through Mapmeister's own MCP tools; turning off "Input files" keeps your data away from it.
- `mapmeister-mcp --session`: a mode for agents that has no tools for creating or writing files; `--no-data` hides the tools that read input.
- Downloads now have stable file names, and each platform has its own update feed.

### 0.3.12 · 18 October 2026

- Generated XSLT matches the engine on large real-world designs: copies through filtered structured results, typed booleans, decimals, date casts, parameter defaults and whole template arguments.
- Calls keep structured parameters and show their connections, and reordering a function's signature no longer swaps a call's connections.
- Components that open under a taller card are moved below it.
- Generated XSLT no longer contains empty `xsl:if` tests for template arguments.

### 0.3.11 · 17 October 2026

- Large designs open readable: structure trees taller than 30 rows scroll inside their component, branches with nothing connected start collapsed, and opening never zooms out below 80%.
- The mouse wheel scrolls the canvas; pinch or Ctrl+wheel zooms.
- XSLT generation through structured function parameters and results.

### 0.3.10 · 17 October 2026

- Function libraries: use the user-defined functions of other .mfd files and the named templates of XSLT stylesheets.
- A design with an empty main canvas opens to a Function library card.
- `mapmeister-cli run --target NAME`.

### 0.3.9 · 17 October 2026

- File ▸ Close (Ctrl+W) closes the open mapping, asking before unsaved changes are discarded.

### 0.3.8 · 17 October 2026

- File ▸ Recent: the last 10 mappings you opened.

### 0.3.7 · 17 October 2026

- Automatic updates: Mapmeister checks for a newer version at start-up, and Help ▸ Check for Updates checks on demand.

### 0.3.6 · 17 October 2026

- XSLT 3.0 generation, including JSON sources and targets. The XSLT button remembers the chosen version.
- REST web service calls. Requests are only made on Run, never in previews.
- Variables from newer MapForce designs import correctly.

### 0.3.5 · 17 October 2026

- New toolbar: labelled insert buttons, Auto-map, Validate, Auto-connect and Live preview, a Run button and an XSLT menu.

### 0.3.4 · 17 October 2026

- Fixed menus stacking into one box when opened.

### 0.3.3 · 17 October 2026

- Menu bar (File, Edit, Insert, Connection, Output, View, Help) above the toolbar.

### 0.3.2 · 17 October 2026

- The Mapmeister view shows constants inline at their ports and lays busy mappings out in columns.
- Selecting components colours their connections, traced back to their sources and on to their targets.

### 0.3.1 · 17 October 2026

- MapForce-style connection editing: Ctrl-drag to copy, move child connections, replace or duplicate inputs, tooltips, alignment guides and highlighted selections.
- Drag either end of a connection to another port.
- Icon toolbar, and in-app dialogs instead of browser prompts.

### 0.3.0 · 17 October 2026

- EDI X12 and EDIFACT components, and MapForce context menus.
- Excel 2007+ (.xlsx) sources and targets.
- Fixed-length text, FlexText templates, HL7 v2, TRADACOMS and SAP IDoc components, and "parse string" components.
- Growable `datetime-add` and `subtract` functions, and typed results of user-defined functions.
- About dialog.
- Wider MapForce import and engine coverage: 73 of MapForce's 135 example outputs now match exactly (up from 29).

### 0.2.0 · 17 October 2026

- Duplicated inputs, variables and joins from MapForce designs, in the engine and the editor.
- A download page for the installers.

### 0.1.0 · 17 October 2026

- First release: visual mapping between XML, JSON and CSV, with functions, lookups, live preview, XSLT generation and MapForce .mfd import.
- Installers for Windows, macOS and Linux, plus `mapmeister-cli` and `mapmeister-mcp`.
