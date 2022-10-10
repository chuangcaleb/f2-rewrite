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

## Build Task

### Motivation

The one Fountain file we work on is very very long, and navigating to specific scenes/sections is quite awkward and clumsy, slowly scrolling until we reach the target. It's just the way Fountain itself works.

I use VS Code and [Better Fountain](https://marketplace.visualstudio.com/items?itemName=piersdeseilligny.betterfountain) as my writing tools. Because of VS Code's versatile features, I have come up with a custom Build Task script that combines (I wouldn't say compile, per se) individual files from a source directory, organized by the principle of separation of responsibilities, into one output file. This repository is a sample showcase of this custom build script.

### Design

The script, [build.py](build.py), simply:

1. Concatenates the content of source files from a specified directory according to a specified order from a configuration list,
2. Adding sufficient newline characters between them, and then
3. Saving them to a specified output path

A YAML file should be used to specify the above configurations, for now, like below. Each key is necessary, for now.

[config.yaml](config.yaml)

```yaml
output_path: ./out/F2Rewrite-Screenplay.fountain
source_dir: ./sections/
structure:
  - title
  - "# ACT 1"
  - lullabie
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

### Further Work

1. Accept other config file types besides YAML, like JSON or TOML
    - Perhaps by default, detect any config filetype in the root working directory, regardless of name, so you could implicitly use "settings.json"
2. Default configuration in the absence of a config file
3. Create multiple output files according to groups of source files, e.g. an output file for each Act, like "Act1.fountain" if the source files are organized like "source/Act1/hook.fountain" and "source/Act1/inciting.fountain".
    - Better configuration key-value structure for explicitly defined groups of source files (probably using lists of lists, but the challenge is intuitively differentiating actual files from group names)
4. Automatic Screenplay to PDF generation or whatever other BetterFountain command, according to post-build tasks specified in the configuration file.
5. Section/Act headers/comments, configured in the config file(?).
6. Perhaps re-code it in a faster language, like Go. But I don't know Go.

---

[Go to my storywriting blog for more details!](https://chuangcaleb.github.io/wtsa/Frozen-II-Rewrite)
