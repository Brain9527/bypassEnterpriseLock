<script setup lang="ts">
import { onMounted, ref } from 'vue';
import Prism from 'prismjs';
import 'prismjs/themes/prism-tomorrow.css';
import 'prismjs/components/prism-bash';

const mdmScript = `#!/bin/bash
RED='\\033[1;31m'
GRN='\\033[1;32m'
BLU='\\033[1;34m'
YEL='\\033[1;33m'
PUR='\\033[1;35m'
CYAN='\\033[1;36m'
NC='\\033[0m'
echo -e "\${CYAN}*-------------------*---------------------*\${NC}"
echo -e "\${YEL}* Check MDM - Skip MDM Auto for MacOS by *\${NC}"
echo -e "\${RED}*      SKIPMDM.COM        *\${NC}"
echo -e "\${RED}*      Phoenix Team        *\${NC}"
echo -e "\${CYAN}*-------------------*---------------------*\${NC}"
echo ""
PS3='Please enter your choice: '
options=("Autoypass on Recovery" "Reboot")
select opt in "\${options[@]}"; do
	case $opt in
	"Autoypass on Recovery")
		echo -e "\${GRN}Bypass on Recovery"
		if [ -d "/Volumes/Macintosh HD - Data" ]; then  
			diskutil rename "Macintosh HD - Data" "Data"
		fi
		echo -e "\${GRN}Create a new user / Tạo User mới"    
		echo -e "\${BLU}Press Enter to continue, Note: Leaving it blank will default to the automatic user" 
		echo -e "Enter the username (Default: Apple) / Nhập tên User (Mặc định: Apple)"
		read realName 
		realName="\${realName:= Apple}"  
		echo -e "\${BLUE}Nhận username \${RED}WRITE WITHOUT SPACES\${GRN} (Mặc định: Apple)"   
		read username
		username="\${username:=Apple}" 
		echo -e "\${BLUE}Enter the password (default: 1234)"  
		read passw   
		passw="\${passw:=1234}"
		dscl_path='/Volumes/Data/private/var/db/dslocal/nodes/Default'     
		echo -e "\${GREEN}Creating User / Đang tạo User" 
		# Create user  
		dscl -f "$dscl_path" localhost -create "/Local/Default/Users/$username"   
		dscl -f "$dscl_path" localhost -create "/Local/Default/Users/$username" UserShell "/bin/zsh"
		dscl -f "$dscl_path" localhost -create "/Local/Default/Users/$username" RealName "$realName"
		dscl -f "$dscl_path" localhost -create "/Local/Default/Users/$username" UniqueID "501"
		dscl -f "$dscl_path" localhost -create "/Local/Default/Users/$username" PrimaryGroupID "20"
		mkdir "/Volumes/Data/Users/$username"
		dscl -f "$dscl_path" localhost -create "/Local/Default/Users/$username" NFSHomeDirectory "/Users/$username"
		dscl -f "$dscl_path" localhost -passwd "/Local/Default/Users/$username" "$passw"
		dscl -f "$dscl_path" localhost -append "/Local/Default/Groups/admin" GroupMembership $username
		echo "0.0.0.0 deviceenrollment.apple.com" >>/Volumes/Macintosh\\ HD/etc/hosts
		echo "0.0.0.0 mdmenrollment.apple.com" >>/Volumes/Macintosh\\ HD/etc/hosts
		echo "0.0.0.0 iprofiles.apple.com" >>/Volumes/Macintosh\\ HD/etc/hosts    
		echo -e "\${GREEN}Successfully blocked host\${NC}"
		touch /Volumes/Data/private/var/db/.AppleSetupDone    
		rm -rf /Volumes/Macintosh\\ HD/var/db/ConfigurationProfiles/Settings/.cloudConfigHasActivationRecord
		rm -rf /Volumes/Macintosh\\ HD/var/db/ConfigurationProfiles/Settings/.cloudConfigRecordFound
		touch /Volumes/Macintosh\\ HD/var/db/ConfigurationProfiles/Settings/.cloudConfigProfileInstalled
		touch /Volumes/Macintosh\\ HD/var/db/ConfigurationProfiles/Settings/.cloudConfigRecordNotFound
		echo -e "\${CYAN}------ Autobypass SUCCESSFULLY ------\${NC}"
		echo -e "\${CYAN}------ Exit Terminal , Reset Macbook and ENJOY ! ------\${NC}"
		break
		;;
	"Exit") 
		echo "Rebooting..."
		reboot
		break
		;;
	*) echo "Invalid option $REPLY" ;;
	esac
done`;

const copyStatus = ref('复制代码');

const handleCopy = () => {
  navigator.clipboard.writeText(mdmScript).then(() => {
    copyStatus.value = '已复制！';
    setTimeout(() => {
      copyStatus.value = '复制代码';
    }, 2000);
  });
};

onMounted(() => {
  Prism.highlightAll();
});
</script>

<template>
  <div class="min-h-screen py-10 px-4 bg-gray-100">
    <div class="max-w-4xl mx-auto bg-white shadow-lg rounded-lg overflow-hidden">
      <!-- Header -->
      <div class="bg-blue-600 py-6 px-8 text-white">
        <h1 class="text-3xl font-bold">苹果 Mac 电脑/MacBook 免费完美绕过监管锁/配置锁教程</h1>
        <p class="mt-2 text-blue-100 italic text-sm">本文代码搬运自知乎程序员 Jay Leo，由 B 站原创润色分享</p>
      </div>

      <!-- Content -->
      <div class="p-8 prose prose-slate max-w-none">
        <div class="bg-red-50 border-l-4 border-red-500 p-4 mb-6">
          <p class="text-red-700 font-bold">⚠️ 强烈建议：购买了货源存疑的 MacBook，收到货后立即尝试联网抹掉磁盘恢复系统！</p>
        </div>

        <h2 class="text-xl font-bold text-gray-800 border-b pb-2 mb-4">一、何谓苹果设备的监管锁（配置锁）</h2>
        <p class="text-gray-700 leading-relaxed mb-4">
          监管锁（MDM/Remote Management）是企业或机构为了对苹果设备进行监管设置的一道屏障。常见于：
        </p>
        <ul class="list-disc pl-5 mb-6 text-gray-700">
          <li>连锁餐饮店用于点餐的 iPad，限制娱乐功能。</li>
          <li>手机租赁平台预设，防止客户逾期不还。</li>
          <li>企业采购给员工使用，限制内网访问或统一安装软件。</li>
        </ul>

        <h2 class="text-xl font-bold text-gray-800 border-b pb-2 mb-4">二、如何解除监管锁</h2>
        <p class="mb-4 text-gray-700">
          <strong>最佳方案：</strong>由上锁的企业或机构解锁。<br>
          <strong>Intel 芯片：</strong>可修改 BIOS 芯片中的主机序列号。<br>
          <strong>Apple M1/M2/M3 芯片：</strong>无法修改序列号，只能采用<strong>绕过</strong>方法。
        </p>

        <h2 class="text-xl font-bold text-gray-800 border-b pb-2 mb-4 text-red-600">三、绕过教程步骤 (M 系列芯片通用)</h2>
        <div class="space-y-4 text-gray-700">
          <p><strong>第一步：</strong>再次联网抹掉磁盘重装系统（必须抹掉磁盘，否则无效）。</p>
          <p><strong>第二步：</strong>复制并执行关键代码：</p>
          <ol class="list-decimal pl-5 space-y-2">
            <li>长按电源键 10 秒硬关机，等待 5 秒。</li>
            <li>长按开机键（指纹键）直到显示“正在载入启动选项”，选择“选项”。</li>
            <li>连接 Wi-Fi，在左上角菜单选择简体中文。</li>
            <li>打开 Safari 浏览器，访问本项目页面并复制下方代码。</li>
            <li>打开终端（在实用工具菜单中）。</li>
            <li>粘贴代码并回车，输入 <code>1</code> (Autoypass on Recovery) 并一路回车。</li>
          </ol>
        </div>

        <!-- Code Block -->
        <div class="relative mt-8 group">
          <div class="flex justify-between items-center bg-gray-800 text-gray-300 px-4 py-2 rounded-t-lg">
            <span class="text-xs font-mono uppercase">Shell Script</span>
            <button 
              @click="handleCopy"
              class="text-xs bg-gray-700 hover:bg-gray-600 px-2 py-1 rounded transition text-white"
            >
              {{ copyStatus }}
            </button>
          </div>
          <pre class="!mt-0 !rounded-t-none"><code class="language-bash">{{ mdmScript }}</code></pre>
        </div>

        <h2 class="text-xl font-bold text-gray-800 border-b pb-2 mt-8 mb-4">四、后续清理与设置</h2>
        <div class="space-y-4 text-gray-700">
          <p><strong>1. 进入临时账户：</strong>重启后输入密码 <code>1234</code> 进入临时账户 Apple。</p>
          <p><strong>2. 创建正式账户：</strong>在“系统偏好设置”中添加一个新的管理员账户，登录新账户并删除原来的 Apple 账户。</p>
          <p><strong>3. 关闭 SIP：</strong>在恢复模式终端输入 <code>csrutil disable</code> 并重启。</p>
          <p><strong>4. 彻底屏蔽监管：</strong>在系统终端中依次执行：</p>
          <pre class="language-bash"><code>sudo rm /var/db/ConfigurationProfiles/Settings/.cloudConfigHasActivationRecord
sudo rm /var/db/ConfigurationProfiles/Settings/.cloudConfigRecordFound
sudo touch /var/db/ConfigurationProfiles/Settings/.cloudConfigProfileInstalled
sudo touch /var/db/ConfigurationProfiles/Settings/.cloudConfigRecordNotFound
sudo launchctl disable system/com.apple.ManagedClient.enroll</code></pre>
          <p><strong>5. 检查状态：</strong>执行 <code>sudo profiles show -type enrollment</code>，若显示 Error 则代表成功。</p>
        </div>
      </div>
      
      <div class="bg-gray-100 p-6 text-center text-gray-500 text-sm">
        <p>本页面仅供学习交流使用，请支持正版软件与合法渠道购买设备。</p>
      </div>
    </div>
  </div>
</template>

<style>
/* 覆盖 prose 的一些默认样式以适配我们的布局 */
.prose pre {
  background-color: transparent !important;
  margin: 0 !important;
  padding: 1rem !important;
}
</style>
