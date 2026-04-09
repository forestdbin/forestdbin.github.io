---
title: Ubuntu GUI Development
date: 2026-04-09 19:29:17
tags:
  - gui
  - gtk
  - qt
  - wxwidgets
  - python
categories:
---

## GTK

```bash
sudo apt install libgtk-3-dev libcairo2-doc freetype2-doc libglib2.0-doc libgtk-3-doc libpango1.0-doc libwayland-doc libx11-doc libxcb-doc devhelp graphicsmagick util-linux-extra glade
sudo apt install libgtk-4-dev libgtk-4-doc gtk-4-examples
```

```c
// hello.c
#include <gtk/gtk.h>

int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);

    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "GTK Hello World");
    gtk_window_set_default_size(GTK_WINDOW(window), 200, 100);

    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);

    gtk_widget_show_all(window);
    gtk_main();

    return 0;
}
```

```bash
gcc hello.c -o hello `pkg-config --cflags --libs gtk+-3.0`
gcc hello.c -o hello `pkg-config --cflags --libs gtk4`
```

## Qt

```bash
sudo apt install cmake meson
sudo apt install qtbase5-dev
sudo apt install qt6-base-dev qtcreator
sudo apt install qt5-doc qtbase5-doc qtbase5-examples qt6-base-doc qt6-base-examples
```

```cpp
// hello.cpp
#include <QApplication>
#include <QPushButton>

int main(int argc, char *argv[]) {
    QApplication app(argc, argv);

    QPushButton button("Qt Hello World");
    button.resize(200, 100);
    button.show();

    return app.exec();
}
```

```bash
g++ hello.cpp -o hello `pkg-config --cflags --libs Qt5Widgets`
g++ hello.cpp -o hello `pkg-config --cflags --libs Qt6Widgets`
```

## wxWidgets

```bash
sudo apt install libwxgtk3.2-dev wx3.2-doc
```

```cpp
// hello.cpp
#include <wx/wx.h>

class MyApp : public wxApp {
public:
    virtual bool OnInit() {
        wxFrame *frame = new wxFrame(NULL, wxID_ANY, "wxWidgets Hello World");
        frame->Show(true);
        return true;
    }
};

wxIMPLEMENT_APP(MyApp);
```

```bash
g++ hello.cpp -o hello `wx-config --cxxflags --libs`
```

## Python binding

```bash
sudo apt install libgirepository-2.0-dev

pip install PyGObject PyQt5 PyQt6 wxPython
```
