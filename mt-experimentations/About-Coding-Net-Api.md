 
> 已经有人总结了更详细的文档(⊙o⊙)哦~
> 
> [http://api-doc.coding.io/](http://api-doc.coding.io/)

github上面的api挺明确，使用也很方便，但是墙内的生活，你们懂，

所以稍微看了一下coding的代码结构，大体上就是这个样子~
	
### Format

	CODING_API = https://coding.net/api
		
* CODING_API/user/USER/project/PROJECT/git/blob/BRANCH/PATH
* CODING_API/user/USER/project/PROJECT/git/treeinfo/BRANCH/PATH


睡觉之前顺便看了一下Coding客户端的源码（好久不用java...现在只会var~ T.T）

	urlProject = String.format(Global.HOST + "/api/user/%s/project/%s", mJumpParam.mUser, mJumpParam.mProject);
	
	String urlBlob = Global.HOST + "/api/user/%s/project/%s/git/blob/master/%s";
    //https://coding.net/api/user/bluishoul/project/AppBubbleDetail/git/blob/master%252F.bowerrc
    
    String urlImage = Global.HOST + "/u/%s/p/%s/git/raw/master/%s";
    //https://coding.net/u/8206503/p/AndroidCoding/git/raw/master/app/src/main/res/drawable-xxhdpi/actionbar_item_normal.png
    
    private String HOST_GIT_TREE = Global.HOST + "/api/user/%s/project/%s/git/tree/master/%s";
    //https://coding.net/api/user/8206503/project/AndroidCoding/git/tree/master
    
    private String HOST_GIT_TREEINFO = Global.HOST + "/api/user/%s/project/%s/git/treeinfo/master/%s";
    
    private String host_git_tree_url = "";
    
    private String host_git_treeinfo_url = "";
    //https://coding.net/api/user/8206503/project/AndroidCoding/git/treeinfo/master
    
	
和看资源加载信息看到的基本一致，只是没加上path~，顺便吐槽一下，NodeJs大法好，java写这东西是有多难！


### NodeJS

这里的不用看了~~，而且代码写的也吐了，已经不用了T.T

    /**
     * Created by thonatos on 14/12/19.
     */
    
    var _docRepo = require('./config').conf.docRepo;
    var _obj = require('../utils/obj');
    
    var CODING = {
        host: 'https://coding.net/api',
        port: 443,
        path: 'user/MTTUSER/project/MTTPROJECT/git/'
    };
    
    //https://coding.net/api/user/thonatos/project/Mt.Notes.And.Documents/git/treeinfo/master/
    
    var GITHUB = {
        host: 'api.github.com',
        port: 443,
        path: '/repos/MTTUSER/MTTPROJECT/contents/'
    };
    
    
    exports.create = {
        docTemplate : function () {
            return {
                templateType : '',
                templateName : ''
            };
        },
        docRepo: function () {
    
            if(_docRepo.GC === "G"){
    
                var _g = _obj.cloneObj(GITHUB);
    
                _g.path = _g.path.replace(/MTTUSER/g,_docRepo.github.doc_user);
                _g.path = _g.path.replace(/MTTPROJECT/g,_docRepo.github.doc_project);
    
                return _g;
    
            }else{
    
                var _c = GITHUB;
                _c.path = _c.path.replace(/MTTUSER/g,_docRepo.coding.doc_user);
                _c.path = _c.path.replace(/MTTPROJECT/g,_docRepo.coding.doc_project);
    
                return _c;
            }
        }
    };

### Example

* /user/thonatos/project/grimrock.org/git/treeinfo/master/service
* /user/thonatos/project/grimrock.org/git/blob/master/app.js
* /user/thonatos/project/grimrock.org/git/blob/master/service/queryService.js

#### Tree

```
{
    "code": 0,
    "data": {
        "infos": [
            {
                "lastCommitMessage": "Init\n",
                "lastCommitDate": 1418543563000,
                "lastCommitId": "4cbeefe7eec0a03a17d04afdfe334ef6028fa375",
                "lastCommitter": {
                    "name": "Thonatos.Yang",
                    "email": "thonatos.yang@gmail.com",
                    "avatar": "https://www.gravatar.com/avatar/281ce5fb158941764bc390600be91610?s=200&d=mm",
                    "link": "mailto:thonatos.yang@gmail.com"
                },
                "mode": "file",
                "path": "service/queryService.js",
                "name": "queryService.js"
            },
            {
                "lastCommitMessage": "Init\n",
                "lastCommitDate": 1418543563000,
                "lastCommitId": "4cbeefe7eec0a03a17d04afdfe334ef6028fa375",
                "lastCommitter": {
                    "name": "Thonatos.Yang",
                    "email": "thonatos.yang@gmail.com",
                    "avatar": "https://www.gravatar.com/avatar/281ce5fb158941764bc390600be91610?s=200&d=mm",
                    "link": "mailto:thonatos.yang@gmail.com"
                },
                "mode": "file",
                "path": "service/renderService.js",
                "name": "renderService.js"
            }
        ]
    }
}
```

#### File

```
{
    "code": 0,
    "data": {
        "ref": "master",
        "can_edit": true,
        "isHead": true,
        "file": {
            "data": "此处省略万字",
            "lang": "javascript",
            "size": 1252,
            "previewed": false,
            "lastCommitMessage": "Init\n",
            "lastCommitDate": 1418543563000,
            "lastCommitId": "4cbeefe7eec0a03a17d04afdfe334ef6028fa375",
            "lastCommitter": {
                "name": "Thonatos.Yang",
                "email": "thonatos.yang@gmail.com",
                "avatar": "https://www.gravatar.com/avatar/281ce5fb158941764bc390600be91610?s=200&d=mm",
                "link": "mailto:thonatos.yang@gmail.com"
            },
            "mode": "file",
            "path": "service/queryService.js",
            "name": "service/queryService.js"
        }
    }
}
```