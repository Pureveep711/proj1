option_1() {
    echo "Do you want to get the Heung-Min Son's data? (y/n) : \c"
    read choice
    if [ "$choice" = "y" ]; then
        son_data=$(awk -F ',' '$1 == "Heung-Min Son" {print $4,$6,$7,$8}' players.csv)
        echo "Team: $son_data"
    fi
}


option_2() {
    echo "What do you want to get the team data of league_position[1~20] : \c"
    read position
    team_data=$(awk -F ',' -v pos="$position" '$6 == pos {print $1,$6,$2/($2+$3+$4)}' teams.csv)
    echo "$team_data"
}


option_3() {
    echo "Do you want to know Top-3 attendance data and average attendance? (y/n) : \c"
    read choice
    if [ "$choice" = "y" ]; then
        top_attendance=$(tail -n +2 matches.csv | sort -t ',' -nrk 2 | head -n 3 | awk -F ',' '{print $3 " vs " $4 " (" $1 ")"}')
        echo "***Top-3 Attendance Match***"
        echo "$top_attendance"
    fi
}


option_4() {
    echo "Do you want to get each team's ranking and the highest-scoring player? (y/n) : \c"
    read choice
    if [ "$choice" = "y" ]; then
        IFS=$'\n'
        for team in $(awk -F ',' '{print $1}' teams.csv | tail -n +2); do
            league_position=$(awk -F ',' -v team="$team" '$1 == team {print $6}' teams.csv)
            top_scorer=$(awk -F ',' -v team="$team" '$4 == team {print $1,$7}' players.csv | sort -rnk 2 | head -n 1)
            echo "$league_position $team $top_scorer"
        done
        unset IFS
    fi
}



option_5() {
    echo "Do you want to modify the format of date? (y/n) : \c"
    read choice
    if [ "$choice" = "y" ]; then
        modified_date=$(awk -F ' - ' '{gsub(/[[:alpha:]]+/,"",$1);gsub(/pm/,"pm",$2);gsub(/am/,"am",$2);print $1,$2}' matches.csv | awk '{gsub(/[[:alpha:]]+/,"",$1);print $1,$2}')
        echo "$modified_date"
    fi
}


option_6() {
    echo "1) Arsenal 11) Liverpool
2) Tottenham Hotspur 12) Chelsea
3) Manchester City 13) West Ham United
4) Leicester City 14) Watford
5) Crystal Palace 15) Newcastle United
6) Everton 16) Cardiff City
7) Burnley 17) Fulham
8) Southampton 18) Brighton & Hove Albion
9) AFC Bournemouth 19) Huddersfield Town
10) Manchester United 20) Wolverhampton Wanderers"
    echo "Enter your team number : \c"
    read team_num
    team_name=$(awk -F ',' -v num="$team_num" '$6 == num {print $1}' teams.csv)
    matches=$(awk -F ',' -v team="$team_name" '$3 == team && $5 > $6 {print $1,$3,$5,"vs",$6,$4}' matches.csv)
    echo "$matches"
}


option_7() {
    echo "Bye!"
    exit 0
}


while :
do
    echo "[MENU]"
    echo "1. Get the data of Heung-Min Son's Current Club, Appearances, Goals, Assists in players.csv"
    echo "2. Get the team data to enter a league position in teams.csv"
    echo "3. Get the Top-3 Attendance matches in mateches.csv"
    echo "4. Get the team's league position and team's top scorer in teams.csv & players.csv"
    echo "5. Get the modified format of date_GMT in matches.csv"
    echo "6. Get the data of the winning team by the largest difference on home stadium in teams.csv & matches.csv"
    echo "7. Exit"

   
    read -p "Enter your CHOICE (1~7) : " choice

    case $choice in
        1)
            option_1
            ;;
        2)
            option_2
            ;;
        3)
            option_3
            ;;
        4)
            option_4
            ;;
        5)
            option_5
            ;;
        6)
            option_6
            ;;
        7)
            option_7
            ;;
        *)
            echo "Invalid option. Please enter a number between 1 and 7."
            ;;
