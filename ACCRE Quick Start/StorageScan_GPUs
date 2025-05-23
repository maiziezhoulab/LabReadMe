
#!/bin/bash

report=storage_report_$(date +'%m_%d_%Y').txt
touch "${report}"

echo "========== STORAGE SUMMARY ==========" >> "${report}"
echo -e "Device\tFilesystem_Type\tSize\tUsed\tAvailable\tUsage\tNode" >> "${report}"
df -hT | grep -E '/maiziezhou_lab|/home' | awk '{printf "%s\t%s\t%s\t%s\t%s\t%s\t%s\n", $1, $2, $3, $4, $5, $6, $7}' >> "${report}"
echo "" >> "${report}"

# Function to scan directories
scan_directory() {
    local label="$1"
    local dir_to_scan="$2"
    echo "========== Detailed Storage Report: ${label} ==========" >> "${report}"
    dir_list=$(ls -l "${dir_to_scan}" 2>/dev/null | awk '/^d/ {print $NF}')
    for i in ${dir_list}; do
        full_path="${dir_to_scan}/${i}"
        echo "---------------------------------------" >> "${report}"
        echo "Directory: ${full_path}" >> "${report}"
        echo "Storage usage:" >> "${report}"
        du -shc "${full_path}" 2>/dev/null >> "${report}"
        echo "Inodes (number of files):" >> "${report}"
        find "${full_path}" -type f 2>/dev/null | wc -l >> "${report}"
    done
    echo "" >> "${report}"
}

# Scan home and lab directories
scan_directory "Home" "/home"
scan_directory "maiziezhou_lab" "/maiziezhou_lab"
scan_directory "maiziezhou_lab2" "/maiziezhou_lab2"
scan_directory "maiziezhou_lab3" "/maiziezhou_lab3"
scan_directory "maiziezhou_lab4" "/maiziezhou_lab4"

chmod 770 "${report}"
echo "Complete. Report saved to ${report}"
