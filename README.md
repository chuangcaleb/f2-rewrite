# Frozen 2 Rewrite

The initial project started in 21 Dec 2019, after watching the movie twice in early December. There was a lot of nearly-there's in the movie, and rather than criticizing the rushed production and clunky dialogue, I wrote a full story/script rewrite.

It was my first time writing and realizing a movie length story, even if an “alternative universe fanfiction”. There were a lot of writing techniques applied and skills developed.

## Key Points

- Aligned and synergized the climaxes of Elsa’s and Anna’s arcs
  - Elsa sees someone else in Ahtohallan
- Refocused the themes and motifs of the story
- Ensured constant ticking-time bomb and antagonistic force:
  - Antagonism personified by a human, Runeard
- Concrete plot/scene objectives
- Kristoff isn’t a himbo anymore, he contributes way more with his subplot arcs with Anna and Mattias
  - Mattias isn’t a plot device, he contributes more
- Made magic out of plot holes
- Reworked the lore
- Fleshed out objectives and conflict of our two factions in the Forest
- Everyone has personal callbacks to F1 during this quest
- We see new opinions on events of Frozen 1
- Cut down on action screen time for story development

## The Screenplay

After discovering [Fountain – A markup language for screenwriting](https://fountain.io/), I needed a story to practice writing with. I also wanted to see the rewrite in screenplay format.

[Go to my storywriting blog for more details!](https://chuangcaleb.github.io/wtsa/Frozen-II-Rewrite)

## Build Task

### Motivation

The one Fountain file we work on is very very long, and navigating to specific scenes/sections is quite awkward and clumsy, slowly scrolling until we reach the target. It's just the way Fountain itself works.

I use VS Code and [Better Fountain](https://marketplace.visualstudio.com/items?itemName=piersdeseilligny.betterfountain) as my writing tools. Because of VS Code's versatile features, I have come up with a custom Build Task script that combines (I wouldn't say compile, per se) individual files from a source directory, organized by the principle of separation of responsibilities, into one output file. This repository is a sample showcase of this custom build script.

### Design

#### Build Script

The script, [build.py](build.py), simply:

1. Concatenates the content of source files from a specified directory according to a specified order from a configuration list,
2. Adding sufficient newline characters between them, and then
3. Saving them to a specified output path

#### Config File

A YAML file should be used to specify the above configurations, for now, like below. Each key is necessary, for now.

[config.yaml](config.yaml)

```yaml
output_path: ./out/F2Rewrite-Screenplay.fountain
source_dir: ./sections/
structure:
  - title
  - "# ACT 1"
  - lullabies
  - i_wants 
  - proposal
  - sleep 
  - answer
  - recovery
  - "# ACT 2A"
  - door
  - forest
  - olaf 
  - encounter
  - campfire
  - giants
  - scarf
  - shipwreck
  - "# ACT 2B"
  - river
  - reindeer
  - taming
  - ahtohallan
  - drowning
  - death
  - "# ACT 3"
  - death
  - dawn
  - destruction
  - rebirth
  - reunion
  - resolution
  - postcreds
```

Right now, the source file names are just the file names without the extensions. One, since they all should be `.fountain` files, it saves needless repetitive typing. Two, it will technically be filetype/extension-agnostic, meaning you could have a `intro.txt` file in your source directory and the build script will parse and include it just like a `.fountain` file. This is because they are all plain text files anyways.

The build script also searches for all files in all subdirectories of the source directory and treats them all the same regardless of relative directory. This means you can organize your source directory however you want, but you probably will organize it according to Acts like how I have.

The great thing about defining the structure order in a config file is that you can use "Move Line Up/Down" hotkeys to quickly reorder scenes by their identifiers, rather than whole blocks of Fountain code. You can also just comment them out to temporarily remove them (again, rather than commenting out whole blocks of Fountain code).

I've made the Fountain title page as its own source file rather as configuration settings in the config file because it's just more native to Fountain, besides you can preview how it looks like in its own Fountain file.

I also have these `# ACT 1` items in the structure list. The build script will watch for structure items that start with the `#` character, and insert the item as a string rather than parsing it as file name. This is so that structure/timeline markers don't need to be inside and attached to a specific Fountain file's scene. You can see how these render in the output file.

#### VS Code Tasks

VS Code has a feature called [Tasks: Custom Tasks](https://code.visualstudio.com/Docs/editor/tasks#_custom-tasks) where we can define a Custom Task.

[.vscode/tasks.json](.vscode/tasks.json)

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Combine Fountain Sections",
      "type": "shell",
      "command": "${command:python.interpreterPath}",
      "args": ["${workspaceFolder}/build.py"],
      "group": {
        "kind": "build",
        "isDefault": true
      },
      "presentation": {
        "echo": true,
        "reveal": "silent",
        "focus": false,
        "panel": "shared",
        "showReuseMessage": false,
        "clear": false
      }
    }
  ]
}
```

Finally, I've simply keybinded the VS Code command "Tasks: Run Build Task" to `Shft+Cmd+B` or whatever you want. I trigger it only when I want to see the the current output, so that it's not always running in the background.

In particular, this following segment makes this task the default built task so that you don't need to navigate through irrelevant other default VS Code Tasks, and you can just run it straight away with "Tasks: Run Build Task".

[.vscode/tasks.json](.vscode/tasks.json)

```yaml
 "group": {
    "kind": "build",
    "isDefault": true
  },
```

#### Conclusion

The resulting working directory is very akin to a typical coding language, with a src directory and a compiled output. Your source directory can be organized however you want, and the config file allows you to add, rearrange and comment in/out scenes/sections very lightning-fast and intuitively. It's a deceptively simple system from the world of coding that just is extensible and scalable to your needs.

### Further Work

1. Is there a way to automatically attach this build script system for Fountain projects, without manually copy-pasting `build.py` and `.vscode/tasks.json` every time?
2. Accept other config file types besides YAML, like JSON or TOML
    - Perhaps by default, detect any config filetype in the root working directory, regardless of name, so you could implicitly use `settings.json`
3. Default configuration in the absence of a config file
4. Create multiple output files according to groups of source files, e.g. an output file for each Act, like `Act1.fountain` if the source files are organized like `source/Act1/hook.fountain` and`source/Act1/inciting.fountain`.
    - Better configuration key-value structure for explicitly defined groups of source files (probably using lists of lists, but the challenge is intuitively differentiating actual files from group names)
5. Automatic Screenplay to PDF generation or whatever other BetterFountain command, according to post-build tasks specified in the configuration file.
6. Section/Act headers/comments, configured in the config file(?).
7. Perhaps re-code it in a faster language, like Go. But I don't know Go.
8. Only scan for plain text formats in the source directory, in case people use other Markdown-based tools with a working directory at the source directory.
9. Custom schema for config

Please open a pull request or an issue if you would like to make improvements!
