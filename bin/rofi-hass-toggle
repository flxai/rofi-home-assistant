#!/usr/bin/env bash
# Toggles state of selected item
json=$(hass-cli -o json state list 2>/dev/null)
idx=$(jq -r '.[] | [.entity_id, .state] | join(" ")' <<< "$json" | column -t | rofi -dmenu -i -markup-rows -format d)
item=$(jq -r '.[].entity_id' <<< "$json" | sed "${idx}q;d")
itype=$(sed -r 's/\..+$//' <<< "$item")

case "$itype" in
    light)
        hass-cli state toggle "$item" &>/dev/null
        ;;
    scene)
        hass-cli service call --arguments entity_id="$item" scene.turn_on &>/dev/null
        ;;
    *)
        notify-send "Error" "Event type '$itype' not implemented yet. Do you have time to file an issue or write a PR?"
        ;;
esac
