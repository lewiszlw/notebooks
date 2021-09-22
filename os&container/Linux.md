#### wc: 统计文本中行数、字数、字符数
-l 统计行数 -c 统计字符数 
`wc \<filename\>`
`cat info.log \| grep "a" \| wc -l`

#### scp: 远程文件拷贝
1.将本地文件复制到远程主机文件夹
`scp local_file remote_username@remote_ip:remote_folder `
2.将远程主机文件复制到本地文件夹
`scp remote_username@remote_ip:remote_filepath local_folder`

#### tcpdump
// TODO