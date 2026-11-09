# Magikarp Ground Mission

**Platform:** picoCTF 2021  
**Category:** General Skills  
**Difficulty:** Easy

## Objective

Connect to the provided challenge container over SSH, navigate between directories, and read three separate flag fragments.

## Approach

### 1. Connect to the challenge

\`ssh ctf-player@chatelaine.cylabacademy.net -p 23233\`

After accepting the host key and entering the challenge password, I was connected to the remote Ubuntu container as \`ctf-player\`.

### 2. Read the first fragment

I listed the files in the starting directory:

\`\`\`bash
ls
\`\`\`

The directory contained \`1of3.flag.txt\` and \`instructions-to-2of3.txt\`. I read both:

\`\`\`bash
cat 1of3.flag.txt
cat instructions-to-2of3.txt
\`\`\`

The instruction said to move to \`/\`, the root directory.

### 3. Move to the root directory

\`\`\`bash
cd /
ls
cat 2of3.flag.txt
cat instructions-to-3of3.txt
\`\`\`

The second fragment was in \`2of3.flag.txt\`. The next instruction told me to go to my home directory using \`~\`.

### 4. Move to the home directory and read the last fragment

I used \`cd\` without an argument, which returns to the current user's home directory:

\`\`\`bash
cd
ls
cat 3of3.flag.txt
\`\`\`

This revealed the third and final fragment.

## Result

Combining the three fragments in order produced the completed flag:

\`\`\`text
academy{xxsh_0ut_0f_//4t3r_47c47679}
\`\`\`

## What I learned

- \`ls\` lists the contents of the current directory.
- \`cat filename\` prints a file's contents.
- \`cd /\` changes the current directory to the filesystem root.
- \`cd\` with no argument takes the user to their home directory; \`cd ~\` is an explicit equivalent.
- The filesystem root \`/\` and a user's home directory \`~\` are different locations.
- A flag split across multiple files must be collected and concatenated in the correct order.

## Key takeaway

This challenge was a practical exercise in basic shell navigation. The important part was following each instruction, recognizing the difference between \`/\` and \`~\`, and reading each fragment from the correct directory.
