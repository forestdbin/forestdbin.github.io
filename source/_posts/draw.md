---
title: 画个图吧
date: 2026-03-07 17:20:27
tags: tool
categories:
---

## Excalidraw

```bash
# install [nodejs](https://nodejs.org/en/download) and yarn
git clone https://github.com/excalidraw/excalidraw.git
cd excalidraw
yarn install
yarn start
# yarn build
```

## Mermaid

```bash
git clone https://github.com/mermaid-js/mermaid-live-editor.git
cd mermaid-live-editor
pnpm install
pnpm dev -- --open
```

## PlantUML

```bash
sudo apt install default-jre
# download [plantuml.jar](https://github.com/plantuml/plantuml/releases/download/v1.2026.1/plantuml-1.2026.1.jar)
java -jar plantuml.jar sample.puml
java -jar plantuml.jar -picoweb
java -jar plantuml.jar -gui
```

```bash
sudo apt install openjdk-17-jdk maven
git clone https://github.com/plantuml/plantuml-server.git
cd plantuml-server
mvn package
mvn jetty:run
```

## Graphviz

```bash
sudo apt install graphviz
sudo apt install xdot
```
