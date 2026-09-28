# Positron extensions guide

This guide helps you to keep only the Positron extensions you need for the course on your droplet. The extensions stay installed in Positron on your laptop, so you can still use them there.

**Before you start**

- You can connect to your droplet from Positron (see [Droplet and SSH setup guide.md](Droplet%20and%20SSH%20setup%20guide.md)).
- Terminal commands are to be run from the terminal in the Positron window that is connected to the droplet.

## Table of Contents

1. [Why remove extensions from the droplet?](#1-why-remove-extensions-from-the-droplet)
2. [Extensions on your laptop and on your droplet](#2-extensions-on-your-laptop-and-on-your-droplet)
3. [Which extensions to keep on the droplet](#3-which-extensions-to-keep-on-the-droplet)
4. [Remove extensions from the droplet](#4-remove-extensions-from-the-droplet)
5. [Install Container Tools on the droplet](#5-install-container-tools-on-the-droplet)
6. [Check the result](#6-check-the-result)
7. [After a Positron update](#7-after-a-positron-update)
8. [Troubleshooting](#8-troubleshooting)

## 1. Why remove extensions from the droplet?

When you connect to your droplet from Positron, Positron runs a server program on the droplet, and the extensions run there too. Every extension that runs uses memory (RAM), and your droplet only has 2 GB. The first time you connect, Positron also installs a set of its standard extensions on the droplet, and we do not use most of them in this course.

On a course droplet with the standard extensions, Positron used about 1 GB of the 2 GB of memory, before any Docker containers were started. When the memory runs out, the droplet becomes slow, or Linux stops a program to free memory, for example one of your containers.

## 2. Extensions on your laptop and on your droplet

Positron keeps two separate sets of extensions:

| Extensions | Where they are installed | Used by |
|---|---|---|
| **Local** | Your laptop | The Positron windows on your laptop |
| **SSH: dvbi** | Your droplet (in `/root/.positron-server/extensions`) | The Positron window that is connected to the droplet |

Extensions that work with your files (for example support for a programming language, or for Docker) run where the files are. In the window that is connected to the droplet, they must therefore be installed on the droplet. Extensions that only change how Positron looks, such as colour themes, run on your laptop and are not installed on the droplet.

This is why you can remove an extension from the droplet and keep it on your laptop: **uninstalling an extension from the droplet only removes the copy on the droplet.**

> [!WARNING]
> Do not use **Disable** on extensions in the window that is connected to the droplet. Positron remembers disabled extensions by their name for all windows, so an extension you disable there is also turned off in Positron on your laptop. Use **Uninstall** as described in section 4.

## 3. Which extensions to keep on the droplet

Keep these extensions on the droplet:

| Extension | Why |
|---|---|
| **Container Tools** (by Microsoft) | Helps you write `Dockerfile` and `compose.yaml` files (colours, suggestions and error checks), and shows your containers, images and volumes in its Containers view |
| **Docker Language Basics** and **YAML Language Basics** | Container Tools needs them, and they are installed together with it |

Remove all other extensions from the droplet. These are the standard extensions Positron installs on the droplet:

| Extension | What it is for |
|---|---|
| **Jupyter**, **Jupyter Keymap**, **Jupyter Notebook Renderers**, **Jupyter Cell Tags** and **Jupyter Slide Show** | Jupyter notebooks. Uninstalling Jupyter also removes the other four |
| **Python Debugger** | Running Python code step by step to find errors |
| **Ruff** | Checking and formatting Python code |
| **Pyrefly - Python Language Tooling** | Checking the types in Python code |
| **Quarto** | Quarto documents and presentations |
| **Shiny** | Shiny apps |
| **Air - R Language Support** | Formatting R code |
| **Posit Publisher** | Publishing content to Posit Connect |
| **Posit Assistant** | Posit's AI assistant |
| **GitHub Pull Requests** | Pull requests and issues on GitHub |
| **Docker** (by Microsoft) | Not needed: it has no features of its own and only installs Container Tools |
| Any extension you have installed on the droplet yourself | Remove it from the droplet too |

Positron's built-in extensions, for example its Python support and Git, are part of Positron. They are not listed in the sections you use below, and you cannot uninstall them.

## 4. Remove extensions from the droplet

1. Connect to your droplet from Positron (see [section 8 in the Droplet and SSH setup guide](Droplet%20and%20SSH%20setup%20guide.md#8-connect-to-the-droplet-from-positron)).
2. Open the Extensions view in the window that is connected to the droplet: click the Extensions icon (four squares) in the bar on the far left, or press `Ctrl+Shift+X` (Windows) or `Cmd+Shift+X` (macOS).
3. Leave the search field at the top of the Extensions view empty. The view now lists the installed extensions in two sections:
   - **LOCAL - INSTALLED**: the extensions on your laptop
   - **SSH: DVBI - INSTALLED**: the extensions on your droplet

   Click the title of a section to fold it in or out.
4. In the **SSH: DVBI - INSTALLED** section, click the gear icon next to an extension you want to remove, and choose **Uninstall**. You can also right-click the extension. **Only uninstall extensions in this section.**
5. Repeat step 4 for every extension in the **SSH: DVBI - INSTALLED** section, except Container Tools, Docker Language Basics and YAML Language Basics.
6. Reload the window, so the removed extensions stop running: open the Command Palette (`Ctrl+Shift+P` on Windows or `Cmd+Shift+P` on macOS) and run `Developer: Reload Window`.

The extensions you removed are now listed under **LOCAL - INSTALLED** with a button **Install in SSH: dvbi**. **Do not click it**, because it installs the extension on the droplet again. For the same reason, do not click the cloud icon "Install Local Extensions in 'SSH: dvbi'..." in the title of the **LOCAL - INSTALLED** section, which installs all of your local extensions on the droplet.

### Alternative: remove the extensions from the terminal

Instead of steps 3-5, you can remove the extensions with one command. Run it in the terminal of the window that is connected to the droplet (the prompt shows `root@<DROPLET_NAME>:~#`):

```bash
S=$(ls -td ~/.positron-server/bin/*/ | head -1)bin/positron-server
for ext in $($S --list-extensions); do
  case $ext in
    ms-azuretools.vscode-containers|vscode.docker|vscode.yaml) echo "Keeping $ext" ;;
    *) $S --uninstall-extension $ext ;;
  esac
done
```

For each extension, the terminal shows "Extension '...' was successfully uninstalled!" or "Keeping ...". The messages "Extension 'ms-toolsai.jupyter-keymap' is not installed" (and the same for the other Jupyter extensions) and "DeprecationWarning" are harmless: the Jupyter extensions were already removed together with Jupyter. Finish with step 6 above (reload the window).

> [!NOTE]
> **What the command does.** It finds the Positron server program on the droplet, asks it for the list of installed extensions, and uninstalls every extension except the three you keep.
>
> <details>
> <summary>Click if you want to see the command explained step by step</summary>
>
> | Part | What it does |
> |---|---|
> | `ls -td ~/.positron-server/bin/*/` | Lists the versions of the Positron server on the droplet, newest first. `-t` sorts by time, and `-d` lists the folders themselves, not what is in them |
> | `head -1` | Keeps only the first line, which is the newest version |
> | `S=$(...)bin/positron-server` | Stores the path of the newest Positron server program in the variable `S`. `$(...)` is replaced by the output of the command in the brackets |
> | `$S --list-extensions` | Lists the IDs of the extensions installed on the droplet, e.g. `quarto.quarto` |
> | `for ext in ...; do ... done` | Runs the lines between `do` and `done` once for each extension, with its ID in the variable `ext` |
> | `case $ext in ... esac` | Compares the ID with the patterns in the lines below it and runs the line of the first pattern that matches |
> | `ms-azuretools.vscode-containers\|vscode.docker\|vscode.yaml)` | The pattern for the three extensions to keep (`\|` means "or"). For these, the command only prints "Keeping" |
> | `*)` | The pattern for all other extensions (`*` matches anything) |
> | `$S --uninstall-extension $ext` | Uninstalls the extension from the droplet |
>
> </details>

## 5. Install Container Tools on the droplet

If Container Tools is not listed in the **SSH: DVBI - INSTALLED** section, install it on the droplet:

1. In the Extensions view of the window that is connected to the droplet, type `Container Tools` in the search field.
2. Choose **Container Tools** by Microsoft and click **Install** (the button may say **Install in SSH: dvbi**).

Docker Language Basics and YAML Language Basics are installed automatically together with it.

## 6. Check the result

In the terminal of the window that is connected to the droplet, list the extensions on the droplet:

```bash
$(ls -td ~/.positron-server/bin/*/ | head -1)bin/positron-server --list-extensions
```

You should see only these three:

```text
ms-azuretools.vscode-containers
vscode.docker
vscode.yaml
```

To see how much memory is free on the droplet, run:

```bash
free -h
```

The column `available` shows how much memory is left for new programs, such as your containers. You can also follow the memory use over time under "Insights" on your droplet's page on DigitalOcean.

Finally, open Positron on your laptop without connecting to the droplet, and open the Extensions view: all your extensions are still installed there.

## 7. After a Positron update

When you update Positron on your laptop, it also updates the server program on the droplet the next time you connect. At the same time, it **installs its standard extensions on the droplet again** (the ones in the table in section 3).

So after each Positron update, check the **SSH: DVBI - INSTALLED** section, and remove the extensions again as described in section 4 (or with the terminal command in section 4).

## 8. Troubleshooting

- **Positron says that an extension cannot be uninstalled because another extension depends on it.** Uninstall the other extension first, and then the one you wanted to remove.
- **You uninstalled an extension from your laptop by mistake** (in the **LOCAL - INSTALLED** section). Open Positron on your laptop without connecting to the droplet, search for the extension in the Extensions view, and click **Install**.
- **An extension you removed is back on the droplet.** Positron has probably been updated (see section 7), or **Install in SSH: dvbi** was clicked. Remove it again as described in section 4.
- **The droplet still uses a lot of memory.** Extensions you uninstall keep running until you reload the window (step 6 in section 4). Your containers also use memory: `docker stats --no-stream` shows how much each running container uses.
