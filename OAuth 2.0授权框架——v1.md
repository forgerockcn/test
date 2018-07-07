---
post_title: 'OAuth 2.0授权框架 - 简介'
author: Bin Yang
layout: post
published: true
---
[fusion_builder_container hundred_percent="no" equal_height_columns="no" menu_anchor="" hide_on_mobile="small-visibility,medium-visibility,large-visibility" class="" id="" background_color="" background_image="" background_position="center center" background_repeat="no-repeat" fade="no" background_parallax="none" parallax_speed="0.3" video_mp4="" video_webm="" video_ogv="" video_url="" video_aspect_ratio="16:9" video_loop="yes" video_mute="yes" overlay_color="" video_preview_image="" border_size="" border_color="" border_style="solid" padding_top="" padding_bottom="" padding_left="" padding_right=""][fusion_builder_row][fusion_builder_column type="1_1" layout="1_1" background_position="left top" background_color="" border_size="" border_color="" border_style="solid" border_position="all" spacing="yes" background_image="" background_repeat="no-repeat" padding_top="" padding_right="" padding_bottom="" padding_left="" margin_top="0px" margin_bottom="0px" class="" id="" animation_type="" animation_speed="0.3" animation_direction="left" hide_on_mobile="small-visibility,medium-visibility,large-visibility" center_content="no" last="no" min_height="" hover_type="none" link=""][fusion_text]

# OAuth 2.0授权框架 - 简介

OAuth2.0授权框架允许第三方应用获取对HTTP服务的有限的访问权限，既可以以资源所有者（resource owner）名义在资源所有者和HTTP服务之间进行允许的交互，也可以允许第三方应用以自己的名义进行访问。

## 1. 传统授权模式
在传统的客户端-服务器身份验证模式中，客户端请求服务器上访问受限的资源，受保护的资源（protected resource）时，需要使用资源所有者的凭据在服务器上进行身份验证。 资源所有者为了给第三方应用提供受限资源的访问权限，需要与第三方共享它的凭据。

```
     +--------+                               +---------------+
     |        |                               |   资源所有者　  |
     |        |                               |               |
     |        |<-(A)-- 共享凭证　　-------------|               |
     | 第三方  |                               +---------------+
     |　应用   |--(B)--+
     |        |       |
     |        |   存储凭证
     |        |       |                       +---------------+
     |        |<------+                       |   资源服务器　  |     
     |        |                               |               |
     |        |--(C)-- 请求访问资源  ---------->|               |
     |        |--(D)-- 许可访问资源  ---------->|               |
     +--------+                               +---------------+

```
                     图1：传统授权模式

* （A）客户端从资源所有者处获得凭据，通常是明文密码。
* （B）客户端将收到的凭据存储在本地。
* （C）客户端使用此凭据在服务器上进行身份验证。      
* （D）服务器验证此凭据，通常是进行密码身份认证，如有效则处理该请求。               

###问题和局限
* 第三方应用需要存储资源所有者的凭据。 
* 服务器需要支持密码身份认证
* 资源所有者的凭据对应过于宽泛，没有时效，不能分割的访问权限。
* 资源所有者不能撤销某个第三方的访问权限而不影响其它第三方，除非更改密码
* 任意第三方应用的非法侵入将间接导致该凭据所保护资源的侵入。


## 2. OAuth授权模式

为了解决这些问题，OAuth引入了授权层，并且区分离开了资源所有者角色和客户端角色。在OAuth中，资源服务器托管着资源所有者的受保护资源。客户端对资源的访问请求将被授予一组不同于资源所有者的凭据，即访问令牌(access token)。这是一组代表特点作用域，生命周期及其他访问属性的字符串。访问令牌由资源所有者认可的授权服务器(authorization server)颁发给第三方。

例如，终端用户（资源所有者）可以许可一个打印服务（客户端）访问其存储在图片分享网站（资源服务器）上的非公开图片（受保护资源）。他不需要提供自己的用户名和密码给该打印服务。而是，他直接与该图片网站信任的服务器（授权服务器）进行身份验证，后者再颁发具体的委托凭据（访问令牌）给该打印服务。

### 2.1. 角色定义
OAuth定义了四种角色：

* 资源所有者(resource owner，RO)

能够许可对受保护资源的访问权限的实体。当资源所有者是个人时，它被称为最终用户(end-user)。

* 资源服务器(resource server, RS)

托管受保护资源的服务器，能够接收和响应使用访问令牌对受保护资源的请求。

* 客户端(client)

凭借资源所有者的授权,代表资源所有者发起对受保护资源的请求的应用程序。

* 授权服务器(authorization server, AS)
在成功验证资源所有者且获得授权后,颁发访问令牌给客户端的服务器。

授权服务器和资源服务器可以是同一台服务器，也可以是分离的不同服务器。授权服务器可以对应一台，或者多台资源服务器。


### 2.2. 协议流程
```
     +--------+                               +---------------+
     |        |--(A)-- 请求授权　　------------>|   资源所有者　  |
     |        |                               |               |
     |        |<-(B)-- 许可授权　　-------------|               |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(C)-- 请求访问令牌             |   授权服务器　  |
     | 客户端  |        (出示授权许可)---------->|               |
     |        |                               |               |
     |        |<-(D)-- 颁发访问令牌 ------------|               |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(E)-- 请求访问资源             |   资源服务器　  |
     |        |        (出示访问令牌)---------->|               |
     |        |                               |               |
     |        |<-(F)-- 许可访问资源 ------------|               |
     +--------+                               +---------------+

```
						图2：OAuth 2.0 抽象协议流程

* （A）客户端从资源所有者处请求授权。授权请求可以直接向资源所有者发起（如图所示），也可以通过授权服务器作为中介间接发起。后者是更可取的方法。
* （B）客户端收到授权许可，这是一个代表资源所有者授权的凭据。该凭据使用本规范中定义的四种许可类型之一表示。
* （C）客户端通过身份认证并出示授权许向授权服务器请求访问令牌。
* （D）授权服务器验证客户端身份并验证授权许可，若有效则颁发访问令牌。
* （E）客户端从资源服务器请求受保护资源并出示访问令牌进行身份验证。
* （F）资源服务器验证访问令牌，若有效则处理该请求。


## 参考资料
1. [RFC6749 - The OAuth 2.0 Authorization Framework](https://tools.ietf.org/html/rfc6749)  
1. [OAuth 2.0 授权框架](https://legacy.gitbook.com/book/yisiqi/the-oauth-2-0-authorization-framework/details)  

[/fusion_text][/fusion_builder_column][/fusion_builder_row][/fusion_builder_container]









