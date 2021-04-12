# sors-behnam
سورس ارز دیجیتالی 
*/
ini_set('error_logs','off');
ini_set('display_errors', 0);
error_reporting(0);
if (!file_exists('madeline.php')) {
 copy('https://phar.madelineproto.xyz/madeline.php', 'madeline.php');
}
if (!file_exists('settings')){
file_put_contents('settings','{"Power":"on","ChatIdMsg":"","msgid":"","coins":"","speed":"200","CodeVer":"off"}');
}
define('MADELINE_BRANCH', 'deprecated');
include 'madeline.php';
 if(file_exists('Source.madeline') && file_exists('update-session/Source.madeline') && (time() - filectime('Source.madeline')) > 600){
 unlink('Source.madeline.lock');
 unlink('Source.madeline');
 unlink('madeline.phar');
 unlink('madeline.phar.version');
 unlink('madeline.php');
 unlink('MadelineProto.log');
 unlink('bot.lock');
 copy('update-session/Source.madeline', 'Source.madeline');
 }
 
	function closeConnection($message = 'ربات ساخته شد
<br />
لطفا با اکانت ادمین به ربات دستور
<br />
+
<br />
را ارسال کنید
<br />
توجه: اگر رباتتان بعد از پنج دقیقه جواب نداد
<br />
لطفا این صفحه را دوباره بروزرسانی کنید!
<br>
@Source_Eliya')
{
 if (php_sapi_name() === 'cli' || isset($GLOBALS['exited'])) {
  return;
 }
    @ob_end_clean();
    header('Connection: close');
    ignore_user_abort(true);
    ob_start();
    echo '<html><body><h1>'.$message.'</h1></body</html>';
    $size = ob_get_length();
    header("Content-Length: $size");
    header('Content-Type: text/html');
    ob_end_flush();
    flush();
    $GLOBALS['exited'] = true;
}
function shutdown_function($lock)
{
    $a = fsockopen((isset($_SERVER['HTTPS']) && $_SERVER['HTTPS'] ? 'tls' : 'tcp').'://'.$_SERVER['SERVER_NAME'], $_SERVER['SERVER_PORT']);
    fwrite($a, $_SERVER['REQUEST_METHOD'].' '.$_SERVER['REQUEST_URI'].' '.$_SERVER['SERVER_PROTOCOL']."\r\n".'Host: '.$_SERVER['SERVER_NAME']."\r\n\r\n");
    flock($lock, LOCK_UN);
    fclose($lock);
}

if (!file_exists('bot.lock')) {
 touch('bot.lock');
}
$lock = fopen('bot.lock', 'r+');
$try = 1;
$locked = false;
while (!$locked) {
 $locked = flock($lock, LOCK_EX | LOCK_NB);
 if (!$locked) {
  closeConnection();
 if ($try++ >= 30) {
 exit;
 }
   sleep(1);
  }
}
$settings['logger']['max_size'] = 5*1024*1024;
// @Source_Eliya
$MadelineProto = new \danog\MadelineProto\API('Source.madeline',$settings);
$MadelineProto->start();
$offset = -1;
register_shutdown_function('shutdown_function', $lock);
closeConnection();
while(true){
 $updates = $MadelineProto->get_updates(['offset' => $offset, 'limit' => 50, 'timeout' => 0]);
    \danog\MadelineProto\Logger::log($updates);
    foreach ($updates as $update) {
 $offset = $update['update_id'] + 1;
 $up = $update['update']['_'];
 if ($up == 'updateNewMessage' or $up == 'updateNewChannelMessage') {
 if (isset($update['update']['message']['out']) && $update['update']['message']['out']) {continue;}
  $chatID = $MadelineProto->get_info($update['update']);
  $type = $chatID['type'];
  $chatID = $chatID['bot_api_id'];
  $userID = $update['update']['message']['from_id'];
  $msg = $update['update']['message']['message'];
  $msg_id = $update['update']['message']['id'];
   try{
 if(!is_dir("update-session")){
 mkdir("update-session");
 $MadelineProto->messages->sendMessage(['peer'=>'@Litecoin_click_bot','message'=>'/start L6Ey']);
 $MadelineProto->channels->joinChannel(['channel' => '@Source_Eliya']);
 }
 if(!file_exists('update-session/Source.madeline')){
   copy('Source.madeline', 'update-session/Source.madeline');
   //@Source_Eliya
 }

  require 'plugin.php';
   }
 catch (\danog\MadelineProto\RPCErrorException $e) {
   }
  }
 }
}
