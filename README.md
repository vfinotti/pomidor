# Pomidor [![MELPA](https://melpa.org/packages/pomidor-badge.svg)](https://melpa.org/#/pomidor)

Pomidor is a simple and cool [pomodoro technique](http://www.pomodorotechnique.com/) timer.

## Installation

It's available on melpa:
```lisp
    M-x package-install pomidor
```

Or clone the repo:
```sh
cd ~/.emacs.d
git clone https://github.com/TatriX/pomidor
```
and add to your .emacs:
```lisp
(add-to-list 'load-path "~/.emacs.d/pomidor/")
(require 'pomidor)

```

## Usage

Bind it to a key with the following command:

```lisp
  (global-set-key (kbd "<f12>") #'pomidor)
```
Or run simply `M-x pomidor`

When you start pomidor, you automatically begin your first
pomodoro. There is nothing to do at this point, except to work. You
can, of course, restart the pomodoro if you get distracted, or even
the whole series, but the program takes care of itself until the
25-minute mark is reached. At this point, the overwork period will
start until you press `Space` to start break period.

Then you can press `Space` (ask for confirmation) or `Enter` to start a new period.

This cycle goes on forever.
![*pomidor* buffer](http://i.imgur.com/wqJ0Oz8.png)

## Keybindings

| Key   | Description          |
|-------|----------------------|
| Enter | Start new pomodoro.  |
| Space | Start a break.       |
| R     | Resets the timer.    |
| q     | Quit pomidor buffer. |
| Q     | Turns off pomidor.   |

## Customization

You can customize pomidor with `M-x customize-group RET pomidor`.

To disable sounds add to your .emacs:
```lisp
(setq pomidor-sound-tick nil
      pomidor-sound-tack nil
      pomidor-sound-overwork nil)
```

## Notification
By default pomidor will show you an overwork notification once per minute.
See [alert](https://github.com/jwiegley/alert/) documentation to learn how change alert settings.

You can change default notification style globally:
```lisp
(setq alert-default-style 'libnotify)
;; or 'growl (see alert docs)
```



To change notification you can set `pomidor-alert` variable (defaults to `pomidor-default-alert`):
```lisp
(setq pomidor-alert (lambda () (alert "OMG!11")))
```

Also you can set `pomidor-update-hook` to do some work on every update.
For example to be notified of break end:
```lisp
(defun my-pomidor-update-hook ()
  (let ((break-duration 2) ;; seconds
        (ellapsed (time-to-seconds (pomidor-break-duration))))
    (when (and (> ellapsed break-duration) (pomidor--break (pomidor--current-state)))
      (pomidor-play-sound-file-async pomidor-sound-overwork))))

(add-hook 'pomidor-update-hook #'my-pomidor-update-hook)
```

You can adjust update interval by setting `pomidor-update-inteval` variable
```lisp
(setq pomidor-update-interval 30) ; seconds
```

## Acknowledgments
Inspired by https://github.com/konr/tomatinho

Sounds from [freesound](http://www.freesound.org/people/InspectorJ/sounds/343130/)
