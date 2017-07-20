# OSS
基于阿里云的OSS使用
1：composer require aliyuncs/oss-sdk-php(或者下载我的压缩包)
2：填写配置项（我写在config.php，这个写在哪里无所谓能引用到就行，就算你写死也没事）
//阿里云OSS配置
    'aliyun_oss' => [
        'KeyId'      => '',  //您的Access Key ID
        'KeySecret'  => '',  //您的Access Key Secret
        'Endpoint'   => '',  //阿里云oss 外网地址endpoint
        'Bucket'     => '',  //Bucket名称
    ],
3：实例化并上传
/**
 * 实例化阿里云OSS
 * @return object 实例化得到的对象
 * @return 此步作为共用对象，可提供给多个模块统一调用
 */
function new_oss(){
    //获取配置项，并赋值给对象$config
    $config=config('aliyun_oss');
    //实例化OSS
    $oss=new \OSS\OssClient($config['KeyId'],$config['KeySecret'],$config['Endpoint']);
    return $oss;
}
4：上传
/**
 * 上传指定的本地文件内容
 *
 * @param OssClient $ossClient OSSClient实例
 * @param string $bucket 存储空间名称
 * @param string $object 上传的文件名称
 * @param string $Path 本地文件路径
 * @return null
 */
function uploadFile($bucket,$object,$Path){
    //try 要执行的代码,如果代码执行过程中某一条语句发生异常,则程序直接跳转到CATCH块中,由$e收集错误信息和显示
    try{
        //没忘吧，new_oss()是我们上一步所写的自定义函数
        $ossClient = new_oss();
        //uploadFile的上传方法
        $ossClient->uploadFile($bucket, $object, $Path);
    } catch(OssException $e) {
        //如果出错这里返回报错信息
        return $e->getMessage();
    }
    //否则，完成上传操作
    return true;
}
5：引用（别忘了）
use OSS\OssClient;
use OSS\Core\OssException;
