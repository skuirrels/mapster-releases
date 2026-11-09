# Mapster change log

Mapster was called Mapmeister up to version 0.3.14.

## 0.8.3 · 9 November 2026

- **Avro schemas split into a file per type.** A schema can now refer to records, enums and fixed types defined in other `.avsc` files by name (`com.example.party.Customer`), as schemas generated one type per file, and Avro's Maven and Gradle plugins, lay them out. Each type comes from the file that defines it: `Customer.avsc` next to the file that refers to it, or in a folder named after its namespace (`com/example/party/`, or a shorter tail such as `party/`) below that folder or one of the eight above it, else any `.avsc` in that folder whose top-level type it is. Those types are written into the schema that goes into `.avro` files and generated code, so other Avro tools can read the files Mapster writes. Before, such a schema failed with "unknown type".
- Errors in split schemas name the file they are in: a type no file defines, a file named after the type that isn't valid JSON, and a type defined in two files (with where it was first defined).
- A run, and code generation, read each Avro schema's files once, however many input files or strings use it.
- In an AI assist session, a schema takes types only from files the session may read (the mapping's folder and the files the mapping uses), so a schema can't pull other files into the conversation.
- Avro container files compressed with `bzip2` or `xz` now read. Avro's encoding, codecs and the text forms of its values now come from a library shared with Tessera; files read and write exactly as before.

## 0.8.2 · 9 November 2026

- **Antigravity for AI assist.** *AI ▸ AI Settings…* can now use Google's Antigravity CLI (`agy`), so requests run on your Google account's plan with Gemini or another model it lists. It gets Mapster's tools through a plugin in the scratch folder, allowed for that run only, and runs headless in its sandbox, so anything else that needs permission is refused. *Sign In…* opens it in a terminal window, since it signs in only interactively there.
- AI requests now run in a new temporary folder for each request that only you can read, instead of a shared, predictable one, so another user on the same computer can't put settings or hooks there for the AI tool to pick up. An answer the tool cut short (a model error or time limit partway through) is now reported as an error rather than shown as complete.

## 0.8.1 · 8 November 2026

- An Avro schema saved as `.json` rather than `.avsc` now opens as an Avro component with that schema. A JSON file only counts as one when it is a named record, enum or fixed type that parses as an Avro schema; anything else still opens as JSON.

## 0.8.0 · 8 November 2026

- Apache Avro sources and targets. Avro components read and write object container files (`.avro`) with the `null`, `deflate`, `snappy` and `zstandard` codecs, or Avro's JSON encoding. Their structure comes from an Avro schema (`.avsc`) or, for a source, from the schema the file holds. Records, arrays, maps, unions, enums, fixed and every logical type map to typed nodes, and decimals keep all their digits. When writing, missing fields take their schema default or null where they can. Add one with Add Structure, or by choosing Apache Avro as a component's format. MapForce has no Avro support.
- Avro components show Avro types in the structure (record, array, map, union, enum, `decimal(9,2)`, timestamp). The Output tab previews an `.avro` file as readable JSON, one object per line, with decimals and dates as text, and says that Save output writes the binary file. Avro's JSON encoding saves as `.json`.
- Avro values keep their meaning through any mapping: explicit nulls (array items, map values, top-level objects, and fields whose default is not null) stay null, decimals wider than 28 digits and doubles keep every digit through field connections, timestamps and dates far from today (such as Java's "forever" value) read and round-trip, and a time with an offset but no seconds converts to UTC. A field given more than one value, a float out of range, an over-long duration, duplicate enum symbols and names that break the Avro naming rules are now errors instead of silent changes. Big-decimals with a negative scale (Java's `1E+3`) read.
- Avro files can't make Mapster use unbounded time or memory: decimals are limited to 10,000 digits, a file to 100 million values (and a million items that hold no data), and a structure to 200,000 nodes.
- Auto-map is better at JSON and Avro. A JSON array's items match by the array's name, so `orders` maps to an Avro `Order` record and nested arrays such as `lines` map to each other. It no longer fills part of a map entry or a nested array whose container found no match, and a recursive node is no longer taken for a value.

## 0.7.3 · 8 November 2026

- Generated XSLT stylesheets have their own XSLT tab beside Output, so they no longer replace the run's output and the two can't be confused. View ▸ XSLT shows them.
- Mapster asks before unsaved changes are lost: on closing the window, Quit, reloading, or opening a file from Finder. Undo now marks the mapping as changed, undoing back to the saved state makes it clean again, and edits made while a save is being written stay unsaved. Undo and Redo always take exactly one step.
- Numbers keep their digits. Large integers, long decimals and JSON numbers pass through exactly (`1.10` stays `1.10`, `12345678901234567890123` is no longer `1.2345678901234568e+22`), and decimal arithmetic and comparison are exact, so `0.1 + 0.2` equals `0.3`.
- Sort no longer crashes when a key mixes numbers and text; numbers sort before text.
- A variable inside a regular user-defined function is computed for each call. Before, every call reused the first call's value.
- An intermediate (chained) document is generated once, so auto-numbers, GUIDs and times match between it and the final target, and web service or database calls in it no longer run twice.
- XML Schema names are resolved by namespace. UBL-style types such as `cbc:IDType`, which extends `udt:IDType`, keep their inherited attributes (`schemeID`), and same-named simple types in two namespaces no longer crash the run.
- Input files that aren't UTF-8 are decoded properly: a byte-order mark or the XML declaration's encoding is honoured, a few stray bytes in a UTF-8 file no longer garble the whole file, and other files are read as Windows-1252.
- EDI dates and implied decimals containing non-ASCII characters no longer crash the run.
- Security: `mapster-cli serve` answers only the local development UI (other pages, other host names and non-JSON requests are refused; `--allow-origin` adds pages). Parameters in a database table's filter or sort order can no longer change the SQL statement. Generated C#, Java and C++ projects escape every name taken from the mapping, so a mapping can't inject code into them. AI assist reads only the mapping's folder and the files the mapping uses.

## 0.7.2 · 8 November 2026

- AI assist sees large mappings. The mapping is shown to the AI in outline (a MapForce mapping that came to over 600,000 characters is now about 20,000), and it looks deeper into a structure only where it needs to. Before, the AI could not see such mappings at all.
- AI assist works inside user-defined functions: it can read a function's body and change it, which is where most of the logic of imported MapForce mappings lives.
- AI assist changes a component in place (a constant's value, a value-map's table, more inputs on a concat) instead of removing and re-adding it.
- A follow-up request in the AI panel builds on the previous answer's changes while they wait to be applied, and applying the newer answer applies both. Before, the earlier changes were silently dropped.
- AI assist no longer picks up your own Claude Code settings, hooks, plugins or `CLAUDE.md`.
- Validate no longer reports required nodes as missing below a node filled as a whole (a copy-all connection or a user function's structured result), nor a missing input file for a function's structured parameter. On a large MapForce mapping this took the list from 258 issues to 23.
- Every button and control shows its description on hover in the desktop app, where macOS never showed it. The two unlabelled toolbar buttons are now described as Tidy view and Highlight selection.

## 0.7.1 · 8 November 2026

- Programs made by `mapster-cli build` on Apple Silicon Macs crashed at once with 0.7.0: `mapster-cli` is now linked with room in its header for the mapping, and `build` refuses a `--runtime` program without that room.
- Connections to the nodes of a user function's structured parameters and results (for example `ShipmentInstruction/ShipmentInstruction`) no longer show "has no input" or "has no output" errors in the Messages pane. A node the parameter's structure does not have is still reported.

## 0.7.0 · 7 November 2026

- Oracle databases. Tables, queries, functions, procedures (also in packages) and PL/SQL blocks work as they do for the other databases, through Oracle Instant Client, which Mapster finds in your home folder, Downloads or `/opt/oracle` on macOS and Windows. MapForce designs with Oracle data sources now run.
- `mapster-cli build mapping -o convert` writes one program with the mapping and the files it reads inside. It takes the options of `mapster-cli run` and needs nothing else installed.
- MongoDB collections as sources, read with a filter, projection, sort and limit set in the component's settings.
- Wildcard input files (`orders/*.xml`) and file names connected to a source's file name port read several files. Each is a document of its own, and the source's file name port gives each file's name, so one output can be written per input file.
- SOAP web services. Give a web service component a WSDL, choose an operation, and its request and response appear as structures; the call is sent in a SOAP envelope, and a SOAP fault stops the run with its message.
- A debugger. Output ▸ Debug Mapping records the run, and the Debugger pane steps through each value computed and each target node written, forwards or backwards, showing the values and the source nodes in context. Breakpoints on components stop it where you need.
- Components move only by their title bar. Press anywhere else on a component (except an expand arrow or a control) and drag to start a connection from that row.

## 0.6.0 · 7 November 2026

- C#, Java and C++ code generation. File ▸ Generate Code (or `mapster-cli codegen --lang csharp|java|cpp`) writes a .NET 10 project, a Maven project or a CMake project with a typed class for the mapping: a property for each parameter and source, and a `Run()` that returns the outputs. The projects run the mapping with Mapster's own engine (the new `mapster_ffi` library, which they include), so their output is the same as Mapster's.
- .NET and Java function libraries. Tools ▸ Add .NET or Java Library… adds a `.dll` or `.jar`, and its public static methods appear in the Libraries pane as functions. They run in a helper process in Run, Live preview and `mapster-cli`; generated C# and Java code calls them directly.
- Global resources. Tools ▸ Global Resources… keeps files, folders, connection strings and text with a value for each configuration, used as `${global:Name}`. The toolbar (or `mapster-cli --config NAME`) picks the configuration, so one mapping can run against test and production files and databases.
- MongoDB collections as targets. Insert ▸ MongoDB Collection… writes documents with insert, update, upsert, replace or delete, typed from a JSON Schema, the collection's validator or sample documents, in one transaction where the server supports it.
- XML output: derived types (`xsi:type`) shown as extra nodes, `xsi:nil` (`set-xsi-nil`, `substitute-missing-with-xsi-nil`, `is-xsi-nil`), CDATA sections, and comments and processing instructions before or after an element. Generated XSLT writes them too.

## 0.5.2 · 6 November 2026

- File ▸ Generate XSLT 1.0, 2.0 and 3.0 work again on mappings with large recursive schemas. Generation used to run for minutes without finishing, so the menu items seemed to do nothing; a mapping such as eDC's UniversalShipment now generates in a few seconds.

## 0.5.1 · 5 November 2026

- Drag a field's name in a structure, or a port's name on a function, to start a connection; you no longer have to hit the small handle. A source starts from its output and a target from its input. The handles themselves are also easier to hit.
- The connection settings of a SQL Server or PostgreSQL database show its server and port as fields of their own. Leave the port empty to use 1433 (SQL Server) or 5432 (PostgreSQL); changing either updates the connection string.
- A named SQL Server instance with a port (`server\instance,15433`) connects straight to that port.
- When a database cannot be reached, the message says whether nothing answered (check the address, port and VPN) or nothing was listening on that port.
- The decision table settings are wider, each column is as wide as its contents, and the column names and rule numbers stay in view while you scroll.

## 0.5.0 · 5 November 2026

- REST web service calls. Insert ▸ Web Service Call (REST)… calls an endpoint with Basic, OAuth 2.0 (client credentials, password, refresh token or a token you already have) or custom header authentication. A call can run once for the whole mapping or once for every input it is fed, and its request and response bodies take a JSON or XML Schema you supply, so they show as structures you can map to and from.
- Database mapping for SQL Server, PostgreSQL and SQLite. Insert ▸ Database Tables… reads tables as a source and writes them as a target with insert, update, merge (insert or update) and delete. Each database runs in one transaction that commits only when the whole mapping succeeds.
- Insert ▸ Database Call (Query, Function, Procedure)… runs an SQL query, a function or a stored procedure. Its parameters appear as inputs, and its outputs come from the result it returns: the rows, output parameters, return value and rows affected.
- SQL Server named instances (`server\instance`) connect through SQL Browser.
- Decision tables. Insert ▸ Decision Table… picks the first rule whose conditions all hold and gives its outputs. A condition can be a value, a comparison, a range such as `10..20`, or several alternatives separated by commas, and `-` matches anything.
- Every file component has a file name port. Connect a value to a target's file name to write one file per item, named from that value; a source's file name port gives the path it read.
- MapForce designs now bring in their database components, SELECT statements, stored procedure and function calls, decision tables and dynamic file names. Database passwords are not imported: the connection reads them from an environment variable such as `${SHIPPING_PASSWORD}`. Oracle databases import but cannot connect yet.

## 0.4.7 · 20 October 2026

- FlexText templates can now be made and changed in Mapster. File ▸ New FlexText Template… and Open FlexText Template…, Edit FlexText Template… on a FlexText component, or Edit… in its settings open a designer. Each container shows the part of the sample text it receives as you edit, and the structure the component will have is shown beside it. Saving writes the `.mft` in MapForce's layout and refreshes the components that use it.
- + Structure… now accepts a FlexText template (`.mft`), and the template field in a FlexText component's settings has a button to choose the file.
- Large XML Schemas load much faster. A recursive type is now expanded once instead of twice, and nodes deeper in the recursion open on demand to any depth. The HL7 v3 example schema goes from 2.5 million nodes to 155 thousand.
- Large structures open quickly on the canvas: branches load when you open them, so opening the HL7 v3 example sends 0.6 MB to the window instead of 20 MB. Running, validating, generating XSLT and saving still use the whole structure.
- The rows of the CSV, EDI and Excel settings in a component's Properties use the full width of the dialog again.

## 0.4.6 · 20 October 2026

- Copy-All connections now show what they copy. Every child item under the connected row has a filled handle, and a line standing off the component joins them to that row, as in MapForce, so you no longer have to open the connection's menu to see that it copies child items.

## 0.4.5 · 19 October 2026

- The installers now include the `mapster-cli` command-line tool, so mappings run from a terminal without Rust or Cargo. Choose Help ▸ Install Command-Line Tool… once to put it on your PATH: on macOS it links `/usr/local/bin/mapster-cli`, on Windows it adds Mapster's folder to your Path, and the Linux `.deb` and `.rpm` already install `/usr/bin/mapster-cli`.
- `mapster-cli run --input FILE` reads the source from FILE instead of the input file the mapping names. Use `--input NAME=FILE` when the mapping has several sources.
- `mapster-cli run --out DIR` and `mapster-cli xslt --out DIR` create the folder when it doesn't exist.

## 0.4.4 · 18 October 2026

- Designs with thousands of connections no longer lock the window. Connections to rows that are scrolled out of view or inside a collapsed branch are drawn as one line to the box's edge or to the branch, and hovering it says how many it stands for. ContainerASN.mfd, with 9,756 connections, now draws 49 lines.

## 0.4.3 · 18 October 2026

- Large structure trees stay responsive. A component's tree now draws only the rows in its scroll box, so expanding a branch of thousands of nodes, or opening a design such as one with 10,000 connections, no longer stalls the window.

## 0.4.2 · 18 October 2026

- Opening a mapping while another is open no longer locks the window. Components of the previous mapping with the same ids were drawn once more with the new mapping's collapsed branches, so a large structure tree opened fully for a moment; on macOS that could take minutes.

## 0.4.1 · 18 October 2026

- The macOS downloads open again. In 0.4.0, macOS reported a downloaded Mapster as damaged and offered only to move it to the Bin; the app is now signed as a whole, so it opens like any app from outside the App Store.

## 0.4.0 · 18 October 2026

- **Mapmeister is now Mapster.** The download site moves to https://skuirrels.github.io/mapster-releases/. Copies of Mapmeister don't update to Mapster: uninstall Mapmeister and install Mapster. View settings, recent files and AI settings start fresh.
- The command-line tools are now `mapster-cli` and `mapster-mcp`; update any MCP client configuration that starts `mapmeister-mcp`.
- EDI configurations go in `~/.mapster/edi` or `MAPSTER_EDI_CONFIG`; `~/.mapmeister/edi` and `MAPMEISTER_EDI_CONFIG` still work.
- `.mmap` mapping files are unchanged.

## 0.3.14 · 18 October 2026

- Running a mapping with a large output no longer locks up the window. The output pane, and the text in the AI consent dialog, now show only the lines in view, so macOS accessibility queries stay fast.

## 0.3.13 · 18 October 2026

- **AI assist:** your own Claude Code or Codex CLI changes the mapping and you apply it. Open the AI panel with Ctrl+Shift+A, or use Fix with AI on a problem in Messages, Fix Problems with AI or Explain with AI. Nothing is sent until AI is on, and the panel lists every change before Apply Changes makes it one undoable step.
- The agent works on a scratch copy of the open mapping through Mapmeister's own MCP tools; turning off "Input files" keeps your data away from it.
- `mapmeister-mcp --session`: a mode for agents that has no tools for creating or writing files; `--no-data` hides the tools that read input.
- Downloads now have stable file names, and each platform has its own update feed.

## 0.3.12 · 18 October 2026

- Generated XSLT matches the engine on large real-world designs: copies through filtered structured results, typed booleans, decimals, date casts, parameter defaults and whole template arguments.
- Calls keep structured parameters and show their connections, and reordering a function's signature no longer swaps a call's connections.
- Components that open under a taller card are moved below it.
- Generated XSLT no longer contains empty `xsl:if` tests for template arguments.

## 0.3.11 · 17 October 2026

- Large designs open readable: structure trees taller than 30 rows scroll inside their component, branches with nothing connected start collapsed, and opening never zooms out below 80%.
- The mouse wheel scrolls the canvas; pinch or Ctrl+wheel zooms.
- XSLT generation through structured function parameters and results.

## 0.3.10 · 17 October 2026

- Function libraries: use the user-defined functions of other .mfd files and the named templates of XSLT stylesheets.
- A design with an empty main canvas opens to a Function library card.
- `mapmeister-cli run --target NAME`.

## 0.3.9 · 17 October 2026

- File ▸ Close (Ctrl+W) closes the open mapping, asking before unsaved changes are discarded.

## 0.3.8 · 17 October 2026

- File ▸ Recent: the last 10 mappings you opened.

## 0.3.7 · 17 October 2026

- Automatic updates: Mapmeister checks for a newer version at start-up, and Help ▸ Check for Updates checks on demand.

## 0.3.6 · 17 October 2026

- XSLT 3.0 generation, including JSON sources and targets. The XSLT button remembers the chosen version.
- REST web service calls. Requests are only made on Run, never in previews.
- Variables from newer MapForce designs import correctly.

## 0.3.5 · 17 October 2026

- New toolbar: labelled insert buttons, Auto-map, Validate, Auto-connect and Live preview, a Run button and an XSLT menu.

## 0.3.4 · 17 October 2026

- Fixed menus stacking into one box when opened.

## 0.3.3 · 17 October 2026

- Menu bar (File, Edit, Insert, Connection, Output, View, Help) above the toolbar.

## 0.3.2 · 17 October 2026

- The Mapmeister view shows constants inline at their ports and lays busy mappings out in columns.
- Selecting components colours their connections, traced back to their sources and on to their targets.

## 0.3.1 · 17 October 2026

- MapForce-style connection editing: Ctrl-drag to copy, move child connections, replace or duplicate inputs, tooltips, alignment guides and highlighted selections.
- Drag either end of a connection to another port.
- Icon toolbar, and in-app dialogs instead of browser prompts.

## 0.3.0 · 17 October 2026

- EDI X12 and EDIFACT components, and MapForce context menus.
- Excel 2007+ (.xlsx) sources and targets.
- Fixed-length text, FlexText templates, HL7 v2, TRADACOMS and SAP IDoc components, and "parse string" components.
- Growable `datetime-add` and `subtract` functions, and typed results of user-defined functions.
- About dialog.
- Wider MapForce import and engine coverage: 73 of MapForce's 135 example outputs now match exactly (up from 29).

## 0.2.0 · 17 October 2026

- Duplicated inputs, variables and joins from MapForce designs, in the engine and the editor.
- A download page for the installers.

## 0.1.0 · 17 October 2026

- First release: visual mapping between XML, JSON and CSV, with functions, lookups, live preview, XSLT generation and MapForce .mfd import.
- Installers for Windows, macOS and Linux, plus `mapmeister-cli` and `mapmeister-mcp`.
