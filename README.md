# cdnfly-kaixin
仅支持CENTOS7
web目录为云端验证文件，请自行搭建

wget https://raw.githubusercontent.com/sqxiaobai-wq/cdnfly-kaixin/main/web/web.tar.gz

tar -zxvf web.tar.gz

0.0.0.0改成(自己搭建的验证服务器IP)
nano /etc/hosts

0.0.0.0  auth.cdnfly.cn

主控登录地址为: http://主控IP/
管理员账号和密码： wenjian/wenjian
普通用户账号和密码： ceshi/ceshi


v5.1.13主控

curl -fsSL https://github.com/sqxiaobai-wq/cdnfly-kaixin/raw/main/master.sh -o master.sh && chmod +x master.sh && ./master.sh --es-dir /home/es

v5.1.16被控

curl -fsSL https://github.com/sqxiaobai-wq/cdnfly-kaixin/raw/main/agent.sh -o agent.sh  && chmod +x agent.sh && ./agent.sh --master-ver v5.1.13 --master-ip  --es-ip  --es-pwd 


## 本仓库相对原版的修改

1. **自托管化**：master.sh、agent.sh、所有安装包（tar.gz）、面板"节点安装命令"、在线升级链路中的下载地址，全部已由原仓库改为本仓库（`sqxiaobai-wq/cdnfly-kaixin`）。
2. **v5.1.13 主控包 - 普通用户账户中心修复**：`panel/src/views/account/` 下 `balance`、`log`、`order` 原为空目录，已改为软链指向 `finance/balance`、`system/log`、`finance/order`，普通用户登录后账户中心的余额/日志/订单页面可正常打开。
3. **v5.1.13 主控包 - 四层转发分组下拉修复**：`views/stream/stream/index.html` 普通用户分支错误地请求了网站分组接口 `/site-groups`，已修正为 `/stream-groups`，并修正选择器 `stream-group-select-add`。
4. **v5.3.6 主控包 - 节点安装命令地址**：`views/system/update/index.html` 中原写死的原作者下载站 `dl2/dl.cdnfly.cn` 已改为本仓库（raw 主地址 + jsdelivr 镜像容灾）。
5. **web 验证端校时接口**：`web/index.php` 的 `/common/datetime` 分支由 `return` 改为 `echo`，主控校时可正常拿到时间。
6. **在线升级脚本**：`upgrade_script/master/50113.sh` 已入库自托管，`web/version.json` 中的升级脚本地址指向本仓库。

> 已安装的主控若需应用上述修复，除账户中心软链（README 顶部说明）外，可在主控服务器执行对应 `sed` 命令替换面板文件；详见本仓库 commit 记录。



