# HammerSpoon Config

* 기존 코드 - <https://github.com/johngrib/hammerspoon-config>
* johngrib님 hammerspoon wiki - <https://johngrib.github.io/wiki/hammerspoon/>

keyboard trigger

origin: f13, f17

mine: f1, f2

## `hs.hotkey.bind`가 사용된곳

* `./modules/clipboard.lua`

```lua
-- line 56
copy = hs.hotkey.bind({"cmd"}, "c", function()
    copy:disable()
    hs.eventtap.keyStroke({"cmd"}, "c")
    copy:enable()
    hs.timer.doAfter(0.1, storeCopy)
end)
```

* `./modules/event_runner.lua`

```lua
-- line 149
    hs.hotkey.bind({}, key, on_mode, off_mode)
```

* `./modules/mouse.lua`

```lua
-- line 167
    hs.hotkey.bind({}, key, on_mouse_mode, off_mouse_mode)
```

* init.lua

```lua
-- line 38
local tabTable = {}

tabTable['Slack'] = {
    left = { mod = {'option'}, key = 'up' },
    right = { mod = {'option'}, key = 'down' }
}
tabTable['Chrome'] = {
    left = { mod = {'control', 'shift'}, key = 'tab' },
    right = { mod = {'control'}, key = 'tab' }
}
tabTable['iTerm2'] = {
    left = { mod = {'control', 'shift'}, key = 'tab' },
    right = { mod = {'control'}, key = 'tab' }
}
tabTable['Code'] = {
    left = { mod = {'command', 'shift'}, key = '[' },
    right = { mod = {'command', 'shift'}, key = ']' }
}
tabTable['_else_'] = {
    left = { mod = {'control'}, key = 'pageup' },
    right = { mod = {'control'}, key = 'pagedown' }
}
```

```lua
-- line 82

local function tabMove(dir)
    return function()
        -- vimlike.close()
        local activeAppName = hs.application.frontmostApplication():name()
        local tab = tabTalble[activeAppName] or tabTable['_else_']
        hs.eventtap.event.newKeyEvent(tab[dir]['mod'], tab[dir]['key'], true):post()
    end
end

hs.hints.hintChars = {
    'q', 'w', 'e', 'r',
    'a', 's', 'd', 'f',
    'z', 'x', 'c', 'v',
    '1', '2', '3', '4',
    'j', 'k',
    'i', 'o',
    'm', ','
}
```

```lua
--line 101
function rapidKey(modifiers, key)
    modifiers = modifiers or {}
    return function()
        hs.eventtap.event.newKeyEvent(modifiers, string.lower(key), true):post()
        hs.timer.usleep(100)
        hs.eventtap.event.newKeyEvent(modifiers, string.lower(key), false):post()
    end
end

local left_event_map = {
    -- hammerspoon 관리
    { key = 'r', mod = {'shift'}, func = hs.reload },
    -- app_toggle
    { key = 'n', mod = {}, func = app_toggle('Notion') },
    { key = 'm', mod = {}, func = app_toggle('Google Meet') },
    { key = 'd', mod = {}, func = app_toggle('dictionary') },
 --   { key = 'p', mod = {}, func = app_toggle('Postman') },
    { key = 'r', mod = {}, func = app_toggle('Reminders') },
    { key = 'h', mod = {}, func = rapidKey({}, 'left') },
    { key = 'j', mod = {}, func = rapidKey({}, 'down') },
    { key = 'k', mod = {}, func = rapidKey({}, 'up') },
    { key = 'l', mod = {}, func = rapidKey({}, 'right') },
    { key = ',', mod = {}, func = tabMove('left') },
    { key = '.', mod = {}, func = tabMove('right') },
}

local right_event_map = {
    -- ''
    -- app_toggle
    { key = ',', mod = {}, func = app_toggle('System Preferences'), msg = 'System Preferences' },
    { key = '/', mod = {}, func = app_toggle('Activity Monitor') },
    --{ key = 'a', mod = {}, func = app_toggle('safari') },
    { key = 'c', mod = {}, func = app_toggle('Google Chrome') },
    { key = 'd', mod = {}, func = app_toggle('discord') },
    { key = 'e', mod = {}, func = app_toggle('Finder') },
    --{ key = 'f', mod = {}, func = app_toggle('Firefox') },
    --{ key = 'g', mod = {}, func = app_toggle('DataGrip') },
    --{ key = 'i', mod = {}, func = app_toggle('IntelliJ IDEA') },
    { key = 'k', mod = {}, func = app_toggle('KakaoTalk') },
    -- { key = 'l', mod = {}, func = app_toggle('Line') },
    -- { key = 'm', mod = {}, func = app_toggle('NoSQLBooster for MongoDB') },
    { key = 'n', mod = {}, func = app_toggle('Notes') },
    -- { key = 'o', mod = {}, func = app_toggle('Microsoft OneNote') },
    { key = 'p', mod = {}, func = app_toggle('Preview') },
    --{ key = 'q', mod = {}, func = app_toggle('Sequel Pro') },
    { key = 'r', mod = {}, func = app_toggle('draw.io') },
    { key = 's', mod = {}, func = app_toggle('Slack') },
    { key = 't', mod = {}, func = app_toggle('Telegram') },
    { key = 'v', mod = {}, func = app_toggle('VimR') },
    { key = 'v', mod = {'shift'}, func = app_toggle('Visual Studio Code') },
    --{ key = 'x', mod = {}, func = app_toggle('Microsoft Excel') },
    { key = 'space', mod = {}, func = app_toggle('iTerm') },
    { key = 'space', mod = {'shift'}, func = app_toggle('Terminal') },
    { key = 'w', mod = {}, func = app_toggle('WezTerm') },
    { key = 'tab', mod = {}, func = hs.hints.windowHints },
    { key = 'tab', mod = {'shift'}, func = hs.window._timed_allWindows },
    -- win_move
    { key = '0', mod = {}, func = win_move.default },
    { key = '0', mod = {'shift'}, func = win_move.move(1/6, 0, 4/6, 1) },
    { key = '1', mod = {}, func = win_move.left_bottom },
    { key = '2', mod = {}, func = win_move.bottom },
    { key = '3', mod = {}, func = win_move.right_bottom },
    { key = '4', mod = {}, func = win_move.left },
    { key = '4', mod = {'shift'}, func = win_move.move(0, 0, 2/3, 1) },
    { key = '5', mod = {}, func = win_move.full_screen },
    { key = '6', mod = {}, func = win_move.right },
    { key = '6', mod = {'shift'}, func = win_move.move(1/3, 0, 2/3, 1) },
    { key = '7', mod = {}, func = win_move.left_top },
    { key = '8', mod = {}, func = win_move.top },
    { key = '9', mod = {}, func = win_move.right_top },
    -- win_move to next screen
    { key = '-', mod = {}, func = win_move.prev_screen },
    { key = '=', mod = {}, func = win_move.next_screen },
    { key = '`', mod = {}, func = win_move.prev_screen },
}
```

```lua
--line 177
do
    local left_event_runner = require('modules.event_runner')
    --left_event_runner:init('f13', left_event_map)
    left_event_runner:init('f1', left_event_map)

    local right_event_runner = require('modules.event_runner')
    --right_event_runner:init('f17', right_event_map)
    right_event_runner:init('f2', right_event_map)
end
```
