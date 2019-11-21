# Swagger3.0
Laravel5.5版本Swagger3.0的部署与应用

- Laravel安装版本5.5
  <br/>composer create-project --prefer-dist laravel/laravel test5 "5.5.*"
  
- [darkaonline/l5-swagger](https://github.com/DarkaOnLine/L5-Swagger) 安装
  <br/>composer require "darkaonline/l5-swagger:5.5.*"
  
- 生成config/l5-swagger.php
  <br/>php artisan vendor:publish --provider "L5Swagger\L5SwaggerServiceProvider"

- .env文件增加
  <br/>L5_SWAGGER_GENERATE_ALWAYS=true
  <br/>SWAGGER_VERSION=3.0

- 配置自动加载swagger服务
  <br/>config/app.php 文件中providers里加入L5Swagger\L5SwaggerServiceProvider::class,
  
- API控制器代码如下
  <br/>
```
       <?php
           namespace App\Http\Controllers\Api;
           use Illuminate\Http\Request;
           use App\Http\Controllers\Controller;
           use DB;
           use Config;
           //@OA\Info仅使用一次
           /**
             * @SWG\Swagger(
             *     @OA\Info(title="接口说明文档", version="1.0")
             * )
            */
            
           class GetBannerController extends Controller
           {
             /**
               * @OA\Get(
               *     path="/api/banner",
               *     tags={"BANNER_API"},
               *     summary="获取广告位接口",
               *     description="获取广告位接口",
               *     operationId="AdShow",
               *     deprecated=false,
               *     @OA\Parameter(
               *         name="position",
               *         in="query",
               *         description="广告位",
               *         required=true,
               *         @OA\Schema(
               *             type="string"
               *         )
               *     ),
               *     @OA\Response(
               *         response=200,
               *         description="操作成功返回"
               *     ),
               *     @OA\Response(
               *         response=400,
               *         description="发生错误"
               *     )
               * )
             **/
               public function index($num){
                 $list = DB::table('banner_images')->where('banner_id','=',$num)->get();
                 $list=AddUrl($list,['pic'],config('wechat.url'));
                 return json_encode($list);
               }

             }
```

-  php artisan l5-swagger:generate 生成文档

-  config/l5-swagger.php 文件下的api/documentation为Swagger-UI的访问路径配置文件
