albumup: 基于fine uploader的相册上传组件
================

###简介###
albumup是一个基于[fine uploader](http://fineuploader.com/)的相册组件，提供了更加友好的图片上传预览功能，可以实现无刷新预览已上传图片，  
并支持用户填写图片说明以及相册说明信息。  

albumup基于jQuery，但也可以更改为基于官方的javascript版本。


###How to use###
*   引入jQuery和 fine uploader必要的css、js文件
*   配置request url
  
        request: {
                  endpoint: '/action/imgupload?attr=album'
                }

*   配置上传按钮  

        text: {
            uploadButton: '<div><i class="icon-upload icon-white"></i> 添加照片</div>'
          }

*   配置模板，以和当前页面风格相融合(这个可以自己改的，不过有点麻烦~)
      
        template: '<div class="qq-uploader clearfix">' +
            '<pre class="qq-upload-drop-area"><span>{dragZoneText}</span></pre>' +
	          '<div class="qq-upload-button btn btn-success">{uploadButtonText}</div>' +
	          '<div class="tips">5M, jpg, png, jpeg</div>'+
	          '<span class="qq-drop-processing"><span>{dropProcessingText}</span>'+
	          '<span class="qq-drop-processing-spinner"></span></span>'+ 
	          '</div>'+
	          '<div><ul class="qq-upload-list"></ul></div>'

*   配置数据响应：albumup每上传一个图像，server都会给client发送一个描述刚上传图片信息的形如    
    {"success":true, "id":"xxxxxx", "url":"yyyyyy"}的json数据，然后client负责更新当前页面  
    来产生缩略图预览，也就是更新整个相册form。
        
        on('complete', function(event, id, fileName, responseJSON) {
          if (responseJSON.success) {
            $('#imgshow').append(
              '<div class="alert item" id="'+responseJSON.id+'">'+
              '<button type="button" class="close" data-dismiss="alert">&times;</button>'+
              '<div class="row">'+
              '<div class="span2">'+
              '<img src="<%=request.getContextPath()%>'+
              responseJSON.url+'" style="height:100px;width:100xp" alt="">'+
              '</div>'+
              '<div class="span4">'+
              '<textarea name="desc" id="'+
              responseJSON.id+"-desc"+'" class="span4 photodesc" rows="5"></textarea>'+ 
              '</div>'+
              '</div>'+
              '</div>');
          }
  
*   当用户保存相册信息时，albumup还是以ajax形式提交一个关于该相册的json数据，等待server返回确认信息。
    相册的json信息类似于：
  
        {
          "albumname":"name", 
          "albumdesc":"album_desc", 
          "photos": [
                      {
                        "id":"1", 
                        "desc":"some desc"
                      },
                    ...]
        }

*   全部配置见[index.jsp](https://github.com/lvwangbeta/albumup/blob/master/WebContent/index.jsp)

###License##

This plugin is open sourced under GNU GPL v3 or you may purchase a Widen Commerical license to 
release you from the terms of GPL v3. 具体见fine uploader [license](https://github.com/Widen/fine-uploader#license)


