# Extract-Words-from-a-website
Extract words from a website and calculate words distribution.


#!/bin/bash
function get_raw() {
	curl https://www.globalrelay.com > temp
}

function remove_script_tag() {
	sed -e 's/<script.*<\/script>//g;/<script/,/<\/script>/{/<script/!{/<\/script>/!d}};s/<script.*//g;s/.*<\/script>//g' temp > temp2
}

function remove_noscript_tag() {
	sed -e 's/<noscript.*<\/noscript>//g;/<noscript/,/<\/noscript>/{/<noscript/!{/<\/noscript>/!d}};s/<noscript.*//g;s/.*<\/noscript>//g' temp2 > temp3
}

function remove_style() {
	sed -e 's/<style.*<\/style>//g;/<style/,/<\/style>/{/<style/!{/<\/style>/!d}};s/<style.*//g;s/.*<\/style>//g' temp3 > temp4
}

function remove_tags() {
	sed -e 's/<[^>]*>//g' temp4 > temp5
}

function remove_punc() {
	cat temp5 | tr -d '[:punct:]' > temp6
}

function count_words() {
	echo $(wc -w temp6)
}
get_raw
remove_script_tag
remove_noscript_tag
remove_style
remove_tags
remove_punc
count_words
rm -f temp temp1 temp2 temp3 temp4 temp5

Bonus question for words distribution.
sed 's/\.//g;s/\(.*\)/\L\1/;s/\ /\n/g' temp6 | sort | uniq -ic
