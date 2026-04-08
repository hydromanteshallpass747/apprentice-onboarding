# Your Workspace

## Setting Up Without Overthinking It

You need three things on your computer to start building with agents. That's it. Not twelve tools, not a complex development environment, not a specific operating system. Three things.

This guide walks you through setting up each one. If you already have any of these installed, skip ahead.

## Tool 1: A Text Editor (VS Code)

You need a place to view and occasionally edit the files your agent creates. The standard choice is **Visual Studio Code** (VS Code). It's free, it works on Windows, Mac, and Linux, and it's what most of the guild uses.

**To install it:**

1. Go to [code.visualstudio.com](https://code.visualstudio.com)
2. Download the version for your operating system
3. Run the installer with the default settings

That's it. You don't need to learn VS Code deeply right now. You need to be able to open files, look at them, and see the file/folder structure of your projects on the left side panel. Everything else you'll pick up as you use it.

**One thing to do after installing:** Open VS Code and look at the left sidebar. There's an icon that looks like two overlapping squares. That's the Extensions panel. Search for and install these three extensions:

- **Markdown Preview Enhanced** - lets you read `.md` files (like this one) with nice formatting
- **Prettier** - automatically makes code files look tidy
- **Live Server** (by Ritwick Dey) - runs HTML projects on a local web server with auto-reload when you save. You'll use this starting with Phase 1 to run your first build in the browser.

These make life easier. Don't go down the rabbit hole of installing fifty extensions. Three is enough.

### Creating and Editing Files in VS Code

You'll spend your time in VS Code looking at files your agent created, and occasionally creating or editing files yourself. A handful of operations cover everything you need.

**Open a folder as a workspace.** VS Code is at its best when you open an entire project folder, not just one file. Go to **File → Open Folder...** and pick the folder you want to work in (later in this chapter you'll create `~/guild-projects/` — that's a good one to open). The folder's contents appear in the **Explorer** panel on the left side. Keep the folder open; you switch projects by opening a different folder.

**Create a new file.** In the Explorer panel on the left, right-click in an empty area (or on a folder name) and choose **New File...**. VS Code drops a text field where you type the filename. Type the name including the extension — `notes.md`, `index.html`, `DESIGN.md`. Press Enter. The file opens in the editor, empty, ready for you to type.

**Save a file.** Press **Ctrl+S** on Windows/Linux or **Cmd+S** on Mac. That's it. If the file has unsaved changes, a small white dot appears next to its name in the tab — the dot becomes an × once you save. Get in the habit of hitting save after every few changes.

**Create a dotfile.** Some filenames start with a dot, like `.gitignore` or `.mcp.json`. These are called dotfiles and most operating systems hide them in regular file browsers, but VS Code handles them without fuss. Use the same **New File...** workflow and type the full name including the leading dot: `.gitignore`. VS Code treats it like any other file.

**Open an existing file.** Click it in the Explorer panel. VS Code opens it in a new tab on the right. You can have many tabs open at once; switch between them by clicking the tab names or with **Ctrl+Tab**.

**Rename a file.** Right-click the file in the Explorer panel → **Rename**. Type the new name, press Enter.

**Delete a file.** Right-click the file in the Explorer panel → **Delete**. Confirm. You can also press **Delete** with the file selected.

That's enough VS Code for the entire curriculum. You do not need to learn the command palette, multi-cursor editing, keyboard shortcuts, or any of the hundreds of other features. When you need one, your agent can tell you how.

## Tool 2: The Terminal

The terminal (also called the command line, command prompt, or shell) is how you talk to your computer using text instead of clicking around. This sounds intimidating, but you only need a handful of commands, and you'll use them the same way every time.

**You already have a terminal.** You don't need to install one.

- **On Mac:** Open the app called "Terminal" (search for it with Spotlight by pressing Cmd+Space and typing "terminal")
- **On Windows:** Open "PowerShell" (search for it in the Start menu). Or install [Windows Terminal](https://aka.ms/terminal) from the Microsoft Store for a nicer experience
- **On Linux:** You already know where your terminal is

**The commands you need to know:**

There are really only six commands you'll use regularly:

`pwd` - **Print Working Directory.** Tells you where you currently are in your computer's file system. Think of it as "where am I right now?" Run this whenever you're not sure where you are.

`ls` - **List.** Shows you all the files and folders in your current location. (On Windows PowerShell, `ls` also works, or you can use `dir`.)

`cd` - **Change Directory.** Moves you into a different folder. `cd Documents` moves you into the Documents folder. `cd ..` moves you back up one level. `cd ~` takes you back to your home folder no matter where you are. (The `~` symbol is shorthand for your home directory. On Mac and Linux that's something like `/home/yourname`. On Windows it's `C:\Users\yourname`. You'll see `~` used throughout this curriculum as a shortcut.)

`mkdir` - **Make Directory.** Creates a new folder. `mkdir my-project` creates a folder called `my-project`.

`cp` - **Copy.** Copies a file from one place to another. `cp file.txt backup.txt` makes a copy of `file.txt` named `backup.txt`.

`clear` - **Clear.** Wipes the terminal screen when it gets cluttered. Doesn't delete anything, just tidies up the display.

That's the list. Six commands. You don't need `grep`, `sed`, `awk`, `chmod`, or any of the hundreds of other commands that exist. If you ever need something more complex, ask an agent for the exact command to run.

**Practice right now:**

Open your terminal and type these commands, one at a time, pressing Enter after each:

```
pwd
ls
mkdir test-folder
cd test-folder
pwd
ls
cd ..
```

You just checked where you were, looked at what was there, created a folder, moved into it, confirmed you moved, saw it was empty (because you just created it), and moved back out. That's the core loop of navigating your computer from the terminal. Everything else builds on this. (You can delete `test-folder` later. It was just for practice.)

## Tool 3: An AI Agent

This is the tool you'll spend most of your time with. There are several options, and this guild is tool-agnostic. The skills you learn here work across all of them. But you need at least one to start.

**Claude** (by Anthropic) - Available at [claude.ai](https://claude.ai) in your browser, or as **Claude Code** which runs in your terminal. Claude is the primary agent used in this guild's examples. The free tier is enough to get started. Claude Code requires a paid API plan but is more powerful for building software because it can directly create and edit files on your computer.

**Cursor** - A code editor (based on VS Code) with an AI agent built in. Available at [cursor.com](https://cursor.com). Good option if you want the agent and editor in one place. Has a free tier.

**Windsurf** - Another AI-powered editor, similar to Cursor. Available at [codeium.com/windsurf](https://codeium.com/windsurf).

**ChatGPT** - Available at [chat.openai.com](https://chat.openai.com). Widely available, good for conversational work, but less integrated with your file system than the other options.

**Our recommendation for starting out:** Begin with Claude at claude.ai. It's free, it's in your browser, and the examples in these guides are written with Claude in mind. As you progress and start building more complex things, you'll likely move to Claude Code or Cursor, but you don't need them yet.

### Opening the Browser's Developer Tools

Every web browser ships with a built-in developer tools panel, and you'll use it to debug your very first Phase 1 project (a web app). Get comfortable opening it now so it's not a new thing at the moment you need it.

**How to open it:**

- **Chrome, Edge, Brave, and most Chromium browsers:** Press **F12**, or right-click anywhere on the page and choose **Inspect**. Keyboard shortcut: **Ctrl+Shift+I** (Windows/Linux) or **Cmd+Option+I** (Mac).
- **Firefox:** Press **F12**, or right-click → **Inspect**. Same shortcuts work.
- **Safari:** You have to enable developer tools first: **Safari** menu → **Settings** → **Advanced** → check **Show features for web developers**. Then right-click → **Inspect Element**, or press **Cmd+Option+I**.

**What you'll see:** A panel opens attached to the bottom or side of the browser window. It has several tabs across the top: **Elements** (or **Inspector**), **Console**, **Sources** (or **Debugger**), **Network**, and others. For the first few phases of the curriculum, you'll only use the **Console** tab.

**The Console tab** is where your page prints messages and errors. When a web app "doesn't work," the first thing you do is open the console and look at it. You'll see three kinds of messages:

- **Red text or a red ❌ icon** — errors. The page hit a problem and stopped doing whatever it was trying to do. The error message tells you roughly what went wrong and which file/line it happened on.
- **Yellow text or a ⚠️ icon** — warnings. Not fatal, but something the browser wants to flag. Usually ignorable for beginner projects.
- **Gray or black text** — informational messages the code printed on purpose (like a `console.log()` call).

**The debugging loop:** When your first build chapter tells you the page is blank or broken, the workflow is:

1. Open the browser.
2. Open devtools (**F12**).
3. Click the **Console** tab.
4. Look for red text.
5. Copy the full red message.
6. Paste it to your agent along with the code and ask for a fix.
7. Apply the fix, save the file, **reload the page** (**F5** or **Ctrl+R** / **Cmd+R**) — devtools do not auto-refresh when you edit source files.
8. Check the console again. Is the red gone? Are there new errors? Repeat.

**One habit to build:** Clear the console between attempts. There's a small 🚫 icon at the top-left of the Console panel that wipes the display. This makes it obvious which messages are from the latest reload versus stale from a previous attempt.

You'll come back to devtools many times throughout the curriculum. The Console tab alone handles 90% of your web debugging. The other tabs exist for later.

## Tool 4: The Rust Toolchain

The guild's default language is Rust. You don't need to learn Rust (the agent handles the syntax), but you do need it installed so you can compile and run the projects you build.

**To install it:**

Go to [rustup.rs](https://rustup.rs) and follow the instructions. The exact path depends on your OS.

**On Mac and Linux**, it's one command in your terminal:

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

When the installer asks which option you want, press **1** for the default installation. Wait for it to finish (a minute or two), then close and reopen your terminal so your shell picks up the new PATH entry.

**On Windows**, there's an extra prerequisite: Rust needs the **Microsoft Visual C++ Build Tools** to actually produce executables, and they don't come with Windows by default. The steps:

1. Download **rustup-init.exe** from [rustup.rs](https://rustup.rs) and double-click it.
2. The installer opens in a terminal window and checks whether the MSVC build tools are present. If they are, skip to step 6.
3. If they're missing, the installer shows a message pointing you to the Visual Studio download page. Install **Visual Studio Installer** from that link.
4. When the Visual Studio Installer opens, check the **Desktop development with C++** workload in the left pane. (Just that one — you don't need any other workloads for Rust.) This is a multi-gigabyte download and can take 15-30 minutes depending on your connection. Click **Install**.
5. Once the C++ build tools are installed, return to the rustup-init window (or re-run rustup-init.exe if you closed it).
6. Press **1** for the default installation.
7. Wait for rustup to finish downloading and installing (another minute or two).
8. **Close and reopen your terminal.** This is critical — the PATH update doesn't take effect until you start a fresh terminal session.

**Alternative for Windows users: WSL2.** If you'd rather use the Linux install path on Windows, install **Windows Subsystem for Linux** (run `wsl --install` in PowerShell as Administrator), pick a Linux distribution (Ubuntu is fine), and do all your guild work inside WSL. This gives you a real Linux terminal, the `curl | sh` install works, and you avoid the MSVC build tools entirely. This is a valid choice for anyone who wants the Linux experience on Windows hardware.

**After installation (all platforms),** confirm it works:

```
rustc --version
cargo --version
```

You should see version numbers for both. `rustc` is the Rust compiler (it turns code into a program you can run). `cargo` is Rust's build tool and package manager (it handles project setup, dependencies, building, and testing). You'll use `cargo` constantly. The agent will tell you when to run commands like `cargo build` or `cargo test`.

**Common Windows error:** If you see `error: linker 'link.exe' not found` the first time you try to build something, it means the MSVC build tools aren't installed or aren't complete. Go back and install the **Desktop development with C++** workload in the Visual Studio Installer. After the install finishes, close and reopen your terminal and try the build again.

### Installing Rust Tools Later

Throughout the curriculum you'll occasionally install extra Rust command-line tools with `cargo install <tool-name>` — the crosslink issue tracker in Phase 2, security scanners like `cargo-audit` and `cargo-deny` in Phase 3. A few things to know so those installs don't confuse you:

- **Where the binary goes.** `cargo install` drops the new binary into `~/.cargo/bin/` (Mac/Linux) or `%USERPROFILE%\.cargo\bin\` (Windows). Rustup adds that folder to your PATH automatically during the initial install, so the new command is available everywhere — but only in shells you open **after** the install.
- **"Command not found" after a successful install.** The #1 cause is that you're still in the terminal you ran `cargo install` in, and the PATH hasn't been re-read. Close and reopen your terminal. If it still doesn't work, check that `~/.cargo/bin` is on your PATH with `echo $PATH` (Mac/Linux) or `echo $env:PATH` (PowerShell).
- **Verifying the install worked.** After any `cargo install`, immediately run the new command with `--version` or `--help`. If it prints something, the install succeeded. If it errors or isn't found, troubleshoot before moving on.
- **Compile times.** `cargo install` compiles the tool from source, which can take anywhere from seconds to several minutes depending on the tool's size. Wait for the final `Installing ~/.cargo/bin/foo` line. Don't ctrl-C out early.

**You don't need to understand Rust yet.** Your first project (the bookmark manager) uses HTML and JavaScript, not Rust. Rust comes in starting with Phase 2. By then you'll be comfortable directing an agent, and the transition will be about the tool, not the workflow.

## Tool 5: Node.js and npm

Node.js is a second language runtime you'll need starting in Phase 3, when the curriculum covers APIs and MCP servers. Several of the tools you'll install later — MCP servers in particular — are distributed through npm, Node.js's package manager. Install it once now so you're not interrupted later.

**To install it:**

Go to [nodejs.org](https://nodejs.org) and download the **LTS** (Long Term Support) version for your operating system. Run the installer with default settings. On Mac and Linux you can alternatively use a version manager like [nvm](https://github.com/nvm-sh/nvm) if you already know what that is; if you don't, just use the installer from nodejs.org.

After installation, close and reopen your terminal, then verify both pieces installed:

```
node --version
npm --version
```

You should see version numbers for both. `node` runs JavaScript programs. `npm` is the package manager that installs tools and libraries (similar to how `cargo` works for Rust).

**What `-g` means.** Later in the curriculum you'll see commands like `npm install -g some-package-name`. The `-g` flag means "global": it installs the package system-wide so you can run it as a terminal command from anywhere. Without `-g`, npm installs into the current folder only, which is the right default for project dependencies but wrong for tools you want to run as commands. When a tool's installation instructions include `-g`, use `-g`.

**You don't need to learn JavaScript.** Just like with Rust, the curriculum is language-agnostic and your agent handles syntax. Node.js is a prerequisite for a few specific tools, not a language you'll study.

## Your Folder Structure

Create a consistent place for your guild work. Open your terminal and run:

```
cd ~
mkdir guild-projects
cd guild-projects
mkdir scratch
```

You now have:

- `~/guild-projects/` - Your main workspace, where your portfolio repository and any other projects will live
- `~/guild-projects/scratch/` - Where you experiment and practice without pressure

Your portfolio repository itself will be created in the next chapter (Git: Just Enough), cloned directly into `~/guild-projects/` as `~/guild-projects/guild-portfolio/`. No nested `portfolio/guild-portfolio/` wrapper — just one folder per project.

This structure isn't mandatory, but having a consistent place to work prevents the chaos of files scattered across your Desktop, Downloads, and random folders.

## Verifying Your Setup

Let's make sure everything works. Do each of these:

1. **Open VS Code.** If it opens without errors, you're good.
2. **Open your terminal.** Type `pwd` and press Enter. You should see a path on your screen. You're good.
3. **Check Rust.** Type `cargo --version`. You should see a version number. You're good.
4. **Check Node.js.** Type `node --version` and then `npm --version`. You should see a version number for each. You're good.
5. **Navigate to your workspace.** Type `cd ~/guild-projects` and then `ls`. You should see `scratch` listed (your portfolio folder gets created in the next chapter). You're good.
6. **Open your agent.** Go to claude.ai (or whichever agent you chose) and type "Hello, I'm setting up my workspace for learning agentic AI development. Can you confirm you can help me build software?" You should get a coherent, helpful response. You're good.

If any of these steps didn't work, ask your agent to help you troubleshoot. Describe exactly what happened: what you typed, what you expected, and what you got instead. This is your first practice at communicating with an agent. Be specific about the problem.

## What You Don't Need

A quick list of things that might seem like prerequisites but aren't:

- **A powerful computer.** If it runs a web browser, it's enough to start. The agents run on remote servers, not on your machine.
- **A second monitor.** Nice to have, not necessary.
- **Specific operating system.** Mac, Windows, and Linux all work fine.
- **Any paid tools.** Everything in Phase 0 and Phase 1 can be done with free tiers.
- **Prior experience.** If you've read this far, you have enough.

You're set up. Next we'll cover just enough git to track your work, and then you're ready to start talking to agents.

---

| Previous | Current | Next |
|:---|:---:|---:|
| [← The Language Landscape](03-the-language-landscape.md) | **Your Workspace** | [Git: Just Enough →](05-git-just-enough.md) |
