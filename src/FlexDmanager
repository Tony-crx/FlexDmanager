#!/bin/bash

# File Manager (Bash Edition v6 - Unified UI)
# Optimized for anti-flicker (buffered), aesthetics, and functionality
# uh well if u open this sh, idk but yeah maybe u want to install this or not
# this software is work in any os soo urm nvm.
# --- Configuration & Colors ---
COLORS=$(tput colors 2>/dev/null || echo 8)
if [[ $COLORS -ge 256 ]]; then
    ORANGE=$(tput setaf 214)
    PURPLE=$(tput setaf 135)
    WHITE=$(tput setaf 15)
    GRAY=$(tput setaf 250)
    RED=$(tput setaf 196)
    GREEN=$(tput setaf 46)
else
    ORANGE=$(tput setaf 3)
    PURPLE=$(tput setaf 5)
    WHITE=$(tput setaf 7)
    GRAY=$(tput setaf 7)
    RED=$(tput setaf 1)
    GREEN=$(tput setaf 2)
fi

RESET=$(tput sgr0)
BOLD=$(tput bold)
BG_BLACK=$(tput setab 0)

# Box Drawing
BOX_H="═"
BOX_V="║"
BOX_TL="╔"
BOX_TR="╗"
BOX_BL="╚"
BOX_BR="╝"
BOX_T_DOWN="╦"
BOX_T_UP="╩"
BOX_T_VERT="║"

CURRENT_DIR=$(pwd)
SELECTED_INDEX=0
SCROLL_OFFSET=0
FOCUS="MAIN" 
CLIPBOARD_FILE=""
STATUS_MSG="Welcome to FlexDmanager"
STATUS_COLOR=$ORANGE
SHORTCUTS=("Home (~)" "Root (/)" "Media (/media)" "Mnt (/mnt)")
SHORTCUT_PATHS=("$HOME" "/" "/media" "/mnt")
SIDEBAR_INDEX=0
FILES=()
LINES=$(tput lines)
COLS=$(tput cols)
trap "tput cnorm; tput sgr0; clear" EXIT
tput civis
tput smcup

# --- Logic ---
get_files() {
    FILES=()
    local count=0
    if [[ "$CURRENT_DIR" != "/" ]]; then
        FILES+=("..")
    fi
    while IFS= read -r -d $'\0' file; do
        if [[ -n "$file" ]]; then
             FILES+=("$file")
             ((count++))
             [[ $count -gt 1000 ]] && break 
        fi
    done < <(find . -maxdepth 1 -mindepth 1 -printf "%f\0" 2>/dev/null | sort -z)
}

change_dir() {
    local target="$1"
    if cd "$target" 2>/dev/null; then
        CURRENT_DIR=$(pwd)
        SELECTED_INDEX=0
        SCROLL_OFFSET=0
        get_files
        STATUS_MSG="Navigated to $CURRENT_DIR"
        STATUS_COLOR=$ORANGE
    else
        STATUS_MSG="Error: Permission denied or invalid path"
        STATUS_COLOR=$RED
    fi
}

draw_horizontal_line() {
    local width=$1
    local char=$2
    if [[ $width -gt 0 ]]; then
        printf "%0.s$char" $(seq 1 $width)
    fi
}

do_copy() {
    local file="${FILES[$SELECTED_INDEX]}"
    if [[ "$file" == ".." ]]; then
        STATUS_MSG="Cannot copy parent directory link"
        STATUS_COLOR=$RED
        return
    fi
    CLIPBOARD_FILE="$CURRENT_DIR/$file"
    STATUS_MSG="Copied: $file"
    STATUS_COLOR=$GREEN
}

do_paste() {
    if [[ -z "$CLIPBOARD_FILE" ]]; then
        STATUS_MSG="Clipboard empty"
        STATUS_COLOR=$RED
        return
    fi
    
    if [[ ! -e "$CLIPBOARD_FILE" ]]; then
        STATUS_MSG="Source file missing"
        STATUS_COLOR=$RED
        CLIPBOARD_FILE=""
        return
    fi
    
    local dest_name=$(basename "$CLIPBOARD_FILE")
    if [[ -e "$dest_name" ]]; then
         STATUS_MSG="File exists. Aborted."
         STATUS_COLOR=$RED
         return
    fi
    
    cp -r "$CLIPBOARD_FILE" .
    if [[ $? -eq 0 ]]; then
        STATUS_MSG="Pasted: $dest_name"
        STATUS_COLOR=$GREEN
        get_files 
    else
        STATUS_MSG="Paste failed"
        STATUS_COLOR=$RED
    fi
}

do_delete_confirm() {
    local file="${FILES[$SELECTED_INDEX]}"
    if [[ "$file" == ".." ]]; then return; fi
    
    draw_modal " DELETE CONFIRMATION " "Delete '$file'?" " (y/n) " $RED
    
    read -rsn1 answer
    if [[ "$answer" == "y" ]]; then
        rm -rf "$file"
        if [[ $? -eq 0 ]]; then
            STATUS_MSG="Deleted: $file"
            STATUS_COLOR=$GREEN
            get_files
            if [[ $SELECTED_INDEX -ge ${#FILES[@]} ]]; then SELECTED_INDEX=$((${#FILES[@]}-1)); fi
        else
            STATUS_MSG="Delete failed"
            STATUS_COLOR=$RED
        fi
    else
        STATUS_MSG="Delete cancelled"
        STATUS_COLOR=$WHITE
    fi
}

do_refresh() {
    get_files
    STATUS_MSG="List refreshed (${#FILES[@]} items)"
    STATUS_COLOR=$WHITE
}
    
show_help() {
    local help_text=("H: Help" "C: Copy" "V: Paste" "R: Remove" "I: Refresh" "Q: Quit")
    local h=${#help_text[@]}
    local w=22
    local y=$((LINES/2 - h/2 - 2))
    local x=$((COLS/2 - w/2))
    local box_color=$PURPLE
    local text_color=$WHITE
    
    tput cup $y $x; echo -ne "${box_color}${BOX_TL}$(draw_horizontal_line $w "$BOX_H")${BOX_TR}${RESET}"
    tput cup $((y+1)) $x; echo -ne "${box_color}${BOX_V}${RESET}${BOLD} HELP COMMANDS        ${box_color}${BOX_V}${RESET}"
    tput cup $((y+2)) $x; echo -ne "${box_color}${BOX_V}$(draw_horizontal_line $w " ")${BOX_V}${RESET}"
    
    for ((i=0; i<h; i++)); do
        tput cup $((y+3+i)) $x
        local str="${help_text[$i]}"
        printf "${box_color}${BOX_V}${text_color} %-21s${box_color}${BOX_V}${RESET}" "$str"
    done
    
    tput cup $((y+3+h)) $x; echo -ne "${box_color}${BOX_BL}$(draw_horizontal_line $w "$BOX_H")${BOX_BR}${RESET}"
    
    read -rsn1 _ 
}


draw_modal() {
    local title="$1"
    local msg="$2"
    local prompt="$3"
    local color="$4"
    
    local w=40
    local h=5
    local y=$((LINES/2 - h/2))
    local x=$((COLS/2 - w/2))
    
    tput cup $y $x; echo -ne "${color}${BOX_TL}$(draw_horizontal_line $w "$BOX_H")${BOX_TR}${RESET}"
    tput cup $((y+1)) $x; printf "${color}${BOX_V}${WHITE}%-40s${color}${BOX_V}${RESET}" " $title"
    tput cup $((y+2)) $x; printf "${color}${BOX_V}${WHITE}%-40s${color}${BOX_V}${RESET}" " $msg"
    tput cup $((y+3)) $x; printf "${color}${BOX_V}${WHITE}%-40s${color}${BOX_V}${RESET}" " $prompt"
    tput cup $((y+4)) $x; echo -ne "${color}${BOX_BL}$(draw_horizontal_line $w "$BOX_H")${BOX_BR}${RESET}"
}


# --- Rendering ---
draw_ui() {
    LINES=$(tput lines)
    COLS=$(tput cols)
    
    local sb_width=$((COLS / 4))
    if [[ $sb_width -lt 20 ]]; then sb_width=20; fi
    local main_width=$((COLS - sb_width - 1))
    local view_height=$((LINES - 4)) 
    
    local border_color=$ORANGE
    if [[ "$FOCUS" == "SIDEBAR" ]]; then
        local left_color=$PURPLE
        local right_color=$ORANGE
    else
        local left_color=$ORANGE
        local right_color=$PURPLE
    fi
    local divider_color=$PURPLE # Shared line always purple? or blend? Make it Purple for Vibe.
    
    tput cup 0 0
    echo -ne "${left_color}${BOX_TL}$(draw_horizontal_line $((sb_width-2)) "$BOX_H")${divider_color}${BOX_T_DOWN}${right_color}$(draw_horizontal_line $((main_width)) "$BOX_H")${BOX_TR}${RESET}"

    # --- Content Rows ---
    for ((i=0; i<view_height-2; i++)); do
        local y=$((i+1))
        
        # --- Sidebar Content ---
        local sb_content=""
        if [[ $i -eq 0 ]]; then
             sb_content="${BOLD} LOCATIONS${RESET}"
        elif [[ $i -ge 2 && $i -lt $((2 + ${#SHORTCUTS[@]})) ]]; then
             local idx=$((i - 2))
             local item="${SHORTCUTS[$idx]}"
             if [[ -n "$item" ]]; then
                 if [[ $idx -eq $SIDEBAR_INDEX && "$FOCUS" == "SIDEBAR" ]]; then
                     sb_content="${PURPLE}> ${item}${RESET}"
                 else
                     sb_content="${GRAY}  ${item}${RESET}"
                 fi
             fi
        fi
        
        # Draw Left Border
        tput cup $y 0
        echo -ne "${left_color}${BOX_V}${RESET}"
        
        # Clear & Print Sidebar
        tput cup $y 1
        draw_horizontal_line $((sb_width - 2)) " "
        tput cup $y 1
        echo -ne "$sb_content"
        
        # Draw Divider
        tput cup $y $((sb_width-1))
        echo -ne "${divider_color}${BOX_T_VERT}${RESET}"
        
        # --- Main Content ---
        local main_content=""
        local file_idx=$((i + SCROLL_OFFSET - 2))
        
        if [[ $i -eq 0 ]]; then
            local display_path="$CURRENT_DIR"
            if [[ ${#display_path} -gt $((main_width - 4)) ]]; then
                display_path="...${display_path: -$((main_width - 7))}"
            fi
            main_content="${BOLD} ${display_path}${RESET}"
        elif [[ $i -ge 2 ]]; then
             if [[ $file_idx -ge 0 && $file_idx -lt ${#FILES[@]} ]]; then
                 local filename="${FILES[$file_idx]}"
                 local display_name="$filename"
                 local limit=$((main_width - 6))
                 
                 if [[ ${#display_name} -gt $limit ]]; then
                     display_name="${display_name:0:$((limit-3))}..."
                 fi
                 
                 local type_ind=" "
                 local color=$WHITE
                 if [[ -d "$filename" || "$filename" == ".." ]]; then
                     type_ind="/"
                     color=$ORANGE
                 fi
                 
                 if [[ $file_idx -eq $SELECTED_INDEX && "$FOCUS" == "MAIN" ]]; then
                     main_content="${PURPLE}> ${type_ind} ${BOLD}${display_name}${RESET}"
                 else
                     main_content="${color}  ${type_ind} ${display_name}${RESET}"
                 fi
             fi
        fi
        
        # Clear & Print Main
        tput cup $y $((sb_width))
        draw_horizontal_line $((main_width)) " "
        tput cup $y $((sb_width))
        echo -ne " $main_content" 
        
        tput cup $y $((COLS-1))
        echo -ne "${right_color}${BOX_V}${RESET}"
    done
    
    # --- Bottom Border ---
    local y_bot=$((view_height-1))
    tput cup $y_bot 0
    echo -ne "${left_color}${BOX_BL}$(draw_horizontal_line $((sb_width-2)) "$BOX_H")${divider_color}${BOX_T_UP}${right_color}$(draw_horizontal_line $((main_width)) "$BOX_H")${BOX_BR}${RESET}"

    # --- Status Bar ---
    tput cup $view_height 0
    tput el
    echo -ne "${STATUS_COLOR} ${STATUS_MSG}${RESET}"

    # --- Footer ---
    tput cup $((LINES-1)) 0
    tput el
    echo -ne "${GRAY} [H] Help  [C] Copy  [V] Paste  [R] Del  [I] Refresh  [Q] Quit${RESET}"
}

# --- Init ---
get_files
# Do NOT force clear on init to avoid flash, but rely on first draw_ui

# --- Loop ---
while true; do
    draw_ui
    read -rsn1 input
    
    if [[ "$input" == "q" ]]; then break; 
    elif [[ "$input" == "h" ]]; then show_help
    elif [[ "$input" == "c" && "$FOCUS" == "MAIN" ]]; then do_copy
    elif [[ "$input" == "v" ]]; then do_paste
    elif [[ "$input" == "r" && "$FOCUS" == "MAIN" ]]; then do_delete_confirm
    elif [[ "$input" == "i" ]]; then do_refresh
    elif [[ "$input" == $'\x1b' ]]; then
        read -rsn2 -t 0.01 input
        case "$input" in
            '[A') 
                 if [[ "$FOCUS" == "MAIN" ]]; then
                    [[ $SELECTED_INDEX -gt 0 ]] && ((SELECTED_INDEX--))
                    [[ $SELECTED_INDEX -lt $SCROLL_OFFSET ]] && ((SCROLL_OFFSET--))
                 else
                    [[ $SIDEBAR_INDEX -gt 0 ]] && ((SIDEBAR_INDEX--))
                 fi
                 ;;
            '[B') 
                 if [[ "$FOCUS" == "MAIN" ]]; then
                    if [[ $SELECTED_INDEX -lt $((${#FILES[@]}-1)) ]]; then
                        ((SELECTED_INDEX++))
                        [[ $SELECTED_INDEX -ge $((SCROLL_OFFSET + LINES - 7)) ]] && ((SCROLL_OFFSET++))
                    fi
                 else
                    [[ $SIDEBAR_INDEX -lt $((${#SHORTCUTS[@]}-1)) ]] && ((SIDEBAR_INDEX++))
                 fi
                 ;;
            '[D') FOCUS="SIDEBAR" ;;
            '[C') FOCUS="MAIN" ;;
        esac
    elif [[ "$input" == "" ]]; then
         if [[ "$FOCUS" == "MAIN" ]]; then
             target="${FILES[$SELECTED_INDEX]}"
             if [[ -d "$target" || "$target" == ".." ]]; then change_dir "$target"; fi
         else
             change_dir "${SHORTCUT_PATHS[$SIDEBAR_INDEX]}"
             FOCUS="MAIN"
         fi
    elif [[ "$input" == $'\t' ]]; then
         [[ "$FOCUS" == "MAIN" ]] && FOCUS="SIDEBAR" || FOCUS="MAIN"
    fi
done
