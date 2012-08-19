# �T�v

ContainerJS�́AJavaScript�A�v���P�[�V�����p�� Dipendency Injection �R���e�i�ł��B

# ����

- **�ˑ��֌W�̉����ƒ���**
 - ContainerJS�́A�R���e�i�Ǘ��Ƃ��ꂽ�I�u�W�F�N�g(=�R���|�[�l���g�ƌĂт܂�)�̐����ƁA���̈ˑ��R���|�[�l���g�̉����E������S���܂��B
     - �ˑ��֌W�́AJavaScript�R�[�h�ɂ��R���|�[�l���g��`�̒��Ŏw�肷�邩�A�N���X���Ő錾�I�ɒ�`�ł��܂��B
     - JavaScript�ɂ̓C���^�[�t�F�C�X���Ȃ��̂ŁA�ˑ��֌W�̉����̓R���|�[�l���g�Ɋ��蓖�Ă�ꂽ���O�ōs���܂��B
 - �ˑ����������R���e�i�ɔC���邱�ƂŁA�ˑ��֌W���l�����C���X�^���X�𐶐����Q�Ƃ�ݒ肷�鏈��(�����郏�C�������O)���������ł��܂��B
 - �R���|�[�l���g�̓R���e�i�ɂ��L���b�V�������̂ŁA�s�v�ȃI�u�W�F�N�g�̐������팸����܂��B

- **���W���[���̒x���ǂݍ��݂ɑΉ�**
 - require.js�ƘA�g���A�K�v�ȃ��W���[����񓯊��Œx���ǂݍ��݂��܂��B
 - ���[�U�[�̑��쓙���󂯂ăR���|�[�l���g�����ۂɎg����܂ŁAJavaScript�\�[�X�̓ǂݍ��݂ƕ]����x���ł��܂��B

- **�A�X�y�N�g�w���v���O���~���O���T�|�[�g**
 - �R���e�i�Ǘ��̃R���|�[�l���g�Ƀ��\�b�h�E�C���^�[�Z�v�^���������߂܂��B
 - ���\�v�����O�̏o�͂ȂǁA�N���X���f�I�ȋ@�\���C���^�[�Z�v�^�ɏW��ł��܂��B

# ���C�Z���X
[New BSD License](http://opensource.org/licenses/BSD-3-Clause) 


# ����m�F�ς݃u���E�U

- IE9+
- GoogleChrome
- Firefox4+

��ECMAScript5�̋@�\���g�p���Ă��܂��B

# �ˑ����W���[��

ContainerJS�͈ȉ��̃��W���[���Ɉˑ����Ă��܂��B

- [RequireJS](http://requirejs.org/) ( [New BSD or MIT License](https://github.com/jrburke/requirejs/blob/master/LICENSE) )

�܂��A�ȉ��̃e�X�e�B���O�t���[�����[�N���g�p���Ă��܂��B

- [jasmine](https://github.com/pivotal/jasmine/) ( [MIT License](https://github.com/pivotal/jasmine/blob/master/MIT.LICENSE) )
- [jasmine-reporters](https://github.com/larrymyers/jasmine-reporters) ( [MIT License](https://github.com/larrymyers/jasmine-reporters/blob/master/LICENSE) )
- [Rhino](https://developer.mozilla.org/en-US/docs/Rhino) ( [MPL 2.0 License](https://developer.mozilla.org/en-US/docs/Rhino/License) )
- [env-js](http://www.envjs.com/)

# Getting started

�uHello World�v���o�͂���T���v���ł��Bsamples/hello-world �ȉ��Ɋ��S�ȃ\�[�X������̂ł�������Q�Ƃ��������B

�t�@�C���\��:

- index.html
- scripts/
    - main.js
    - require.js
    - container.js
    - app/
        - model.js
        - view.js
    - utils/
        - observable.js

scripts/app/model.js:

    define(["utils/observable"], function(Observable){
    
         "use strict";
    
         /**
         * @class
         */
        var Model = function() {};
        
        Model.prototype = new Observable();
        
        /**
         * @public
         */
        Model.prototype.initialize = function() {
            this.fire( "updated", { 
                property: "message", 
                value :"hello world."
            });
        };
    
        return Model;
    });

scripts/app/view.js:

    define(["container"], function( ContainerJS ){
        
        "use strict";
        
        /**
         * @class
         */
        var View = function() {
            this.model = ContainerJS.Inject("app.Model");
        };
        
        /**
         * @public
         */
        View.prototype.initialize = function() {
            this.model.addObserver("updated", function( ev ) {
                if ( ev.property != "message" ) return;
                var e = document.getElementById(this.elementId);
                e.innerHTML = ev.value; 
            }.bind(this));
        };
        
        return View;
    });

scripts/app/main.js:

    require.config({
        baseUrl: "scripts",
    });
    require(["container"], function( ContainerJS ) {
        
        var container = new ContainerJS.Container( function( binder ){
            
            binder.bind("app.View").withProperties({
                elementId : "console"
            }).onInitialize("initialize")
            .inScope(ContainerJS.Scope.EAGER_SINGLETON);
            
            binder.bind("app.Model");
            
        });
        
        window.addEventListener("load", function() {
            container.get("app.Model").then(function( model ){
                model.initialize();
            }, function( error ) {
                alert( error.toString() ); 
            });
        }, false);
        
    });

index.html:

    <!DOCTYPE html>
    <html>
      <head>
        <meta charset="utf-8">
        <title>Hello World</title>
        <script type="text/javascript" data-main="scripts/main.js" src="scripts/require.js"></script>
      </head>
      <body>
        <div id="console"></div>
      </body>
    </html>

# ���t�@�����X

## Binding

�ȉ���5�̎�i�ł̃R���|�[�l���g�o�^���T�|�[�g���Ă��܂��B

- �N���X�w��
- �v���g�^�C�v�w��
- �I�u�W�F�N�g�w��
- �v���o�C�_�w��
- �C���X�^���X�w��

### �N���X�w��

- �R���|�[�l���g���N���X(�R���X�g���N�^�֐�)�Ŏw�肵�܂��B
- �R���X�g���N�^�֐��� `new` ���Z�q�t���ŌĂяo���č쐬���ꂽ�I�u�W�F�N�g���R���|�[�l���g�ƂȂ�܂��B
- �R���X�g���N�^�֐��́Arequire.js�� `require()` ���g�p���Ĕ񓯊��ǂݍ��݂���܂��B
- `withConstructorArgument()` �ŁA�R���X�g���N�^�֐��ɓn���������ЂƂ����w��ł��܂��B 

�R���|�[�l���g��`:

    var container = new ContainerJS.Container( function( binder ){
        binder.bind("app.Class");
        binder.bind("anotherName").to("app.Class").withConstructorArgument({
            foo:"foo",
            var:ContainerJS.Inject("app.Class") // �ˑ����������\
        });
    });

app/class.js:

    define(function(){
        /**
         * @class
         */
        var Class = function(arg) { 
            this.foo = args.foo;
            this.var = args.var;
        };
        return Class;
    });


### �v���g�^�C�v�w��

- �R���|�[�l���g���v���g�^�C�v�Ŏw�肵�܂��B
- �v���g�^�C�v�������Ƃ��āA`Object#create()`�ō쐬���ꂽ�I�u�W�F�N�g���R���|�[�l���g�ƂȂ�܂��B
- �v���g�^�C�v�́Arequire.js�� `require()` ���g�p���Ĕ񓯊��ǂݍ��݂���܂��B

�R���|�[�l���g��`:

    var container = new ContainerJS.Container( function( binder ){
        binder.bind("app.Prototype").asProrotype();
        binder.bind("anotherName").toPrototype("app.Prototype", {
           foo : { value: "foo" } // ��2������Object#create()�ɓn���������w��ł��܂�
        });
    });

app/prototype.js:

    define(function(){
        /**
         * @class
         */
        var Prototype = {
            method : function( arg ) {
                return arg;
            }
        }
        return Prototype;
    });

### �I�u�W�F�N�g�w��

- require.js��`require` ���g�p���ă��[�h���ꂽ�I�u�W�F�N�g���̂��̂��R���|�[�l���g�ɂ��܂��B

�R���|�[�l���g��`:

    var container = new ContainerJS.Container( function( binder ){
        binder.bind("app.Object").asObject();
        binder.bind("anotherName").toObject("app.Object");
    });

app/object.js:

    define(function(){
        var Obj = {
            method : function( arg ) {
                return arg;
            }
        }
        return Obj;
    });


### �v���o�C�_�w��

- �R���|�[�l���g�𐶐�����֐����w�肵�܂��B
- �֐��̖߂�l���R���|�[�l���g�ƂȂ�܂��B

�R���|�[�l���g��`:

    var container = new ContainerJS.Container( function( binder ){
        binder.bind("name").toProvider(function(){
            return "foo";
        });
    });

### �C���X�^���X�w��

- �R���|�[�l���g���̂��̂��w�肵�܂��B

�R���|�[�l���g��`:

    var container = new ContainerJS.Container( function( binder ){
        binder.bind("name").toInstance("foo");
    });

## Packaging Policy

Packaging Policy��ݒ肷�邱�ƂŁA���W���[���̓ǂݍ��ݐ�𐧌�ł��܂��B

### MODULE\_PER\_CLASS

�f�t�H���g�̃|���V�[�ł��B�N���X���ƂɃ��W���[�����p�ӂ���Ă���Ƃ݂Ȃ��܂��B
�R���|�[�l���g�́A���O��ԂƓ����p�X�ȉ��� `<�N���X����-��؂�ɂ�������>.js` ����ǂݍ��܂�܂��B 

�t�@�C���\��:

- app/
    - foo/
        - hoge-hoge.js
        - fuga-fuga.js
- main.js

app/foo/hoge-hoge.js:

    define(function(){
        /**
         * @class
         */
        var HogeHoge = function(arg) {};
        return HogeHoge;
    });

app/foo/fuga-fuga.js:

    define(function(){
        /**
         * @class
         */
        var FugaFuga = function(arg) {};
        return FugaFuga;
    });

main.js:

    var container = new ContainerJS.Container( function( binder ){
        binder.bind("app.foo.HogeHoge");
        binder.bind("app.foo.FugaFuga");
    });

### MODULE\_PER\_PACKAGE

�p�b�P�[�W���ƂɃ��W���[�����p�ӂ���Ă���Ƃ݂Ȃ��܂��B

�t�@�C���\��:

- app/
    - foo.js
- main.js

app/foo.js:

    define(function(){
    
        /**
         * @class
         */
        var HogeHoge = function(arg) {};
        
        /**
         * @class
         */
        var FugaFuga = function(arg) {};
        
        return {
            HogeHoge : HogeHoge,
            FugaFuga : FugaFuga
        }
    });

main.js:

    var container = new ContainerJS.Container( function( binder ){
        binder.bind("app.foo.HogeHoge")
            .assign(ContainerJS.PackagingPolicy.MODULE_PER_PACKAGE);
        binder.bind("app.foo.FugaFuga")
            .assign(ContainerJS.PackagingPolicy.MODULE_PER_PACKAGE);
    });

### SINGLE_FILE

1�̃t�@�C���ɖ��O��ԓ��̂��ׂẴN���X����`����Ă���Ƃ݂Ȃ��܂��B

�t�@�C���\��:

- app.js
- main.js

app.js:

    define(function(){
    
        /**
         * @class
         */
        var HogeHoge = function(arg) {};
        
        /**
         * @class
         */
        var FugaFuga = function(arg) {};
        
        return {
            foo : {
                HogeHoge : HogeHoge,
                FugaFuga : FugaFuga
            }
        }
    });

main.js:

    var container = new ContainerJS.Container( function( binder ){
        binder.bind("app.foo.HogeHoge")
            .assign(ContainerJS.PackagingPolicy.SINGLE_FILE);
        binder.bind("app.foo.FugaFuga")
            .assign(ContainerJS.PackagingPolicy.SINGLE_FILE);
    });

## Scope

�uSingleton�v,�uEagerSingleton�v,�uPrototype�v���T�|�[�g���܂��B�f�t�H���g�́uSingleton�v�ł��B

- **Singleton**
    - �R���|�[�l���g��1�����������܂��B
    - �����񓯂��R���|�[�l���g���擾�����ꍇ�A��ɏ����get()���ɍ쐬�����R���|�[�l���g���Ԃ���܂��B
    - �R���|�[�l���g�� `Container#destroy()` �Ŕj������܂��B
- **EagerSingleton**
    - Singleton�Ɠ������B��̃C���X�^���X��Ԃ��܂����A�C���X�^���X���R���e�i�̍쐬���ɐ�������܂��B(Singleton�̏ꍇ�A�R���e�i���珉�߂ăR���|�[�l���g���擾�����ۂɐ�������܂��B)
    - ������g���ƁA�R���e�i�ɓo�^���Ă��������Ō��ʂ𔭊�����R���|�[�l���g���쐬�ł��܂��B
- **Prototype**
    - �R���|�[�l���g�擾�̂��тɁA�R���|�[�l���g���č쐬���܂��B

�ݒ�̕ύX�́A `inScope()` �ōs���܂��B

    var container = new ContainerJS.Container( function( binder ){
        binder.bind("Foo").inScope( ContainerJS.Scope.SINGLETON ); // default
        binder.bind("Bar").inScope( ContainerJS.Scope.EAGER_SINGLETON );
        binder.bind("Val").inScope( ContainerJS.Scope.PROTOTYPE );
    });

## Injection

�v���p�e�B�� `ContainerJS.Inject` ��ݒ肵�Ă����ƁA�R���e�i�ɂ��ˑ����W���[�����T�����꒍������܂��B

- `ContainerJS.Inject` ��ݒ肷��ƁA�v���p�e�B���ŃR���|�[�l���g���T������܂��B
- `ContainerJS.Inject(name)` �ŁA�T������R���|�[�l���g���𖾎��ł��܂��B
- `ContainerJS.Inject.all`,`ContainerJS.Inject.all(name)` �ŁA�w�肳�ꂽ���O�����R���|�[�l���g�̔z�񂪒�������܂��B
- `ContainerJS.Inject.lazily`,`ContainerJS.Inject.lazily(name)`,`ContainerJS.Inject.all.lazily`,`ContainerJS.Inject.all.lazily(name)` �ŁA���������R���|�[�l���g���x���ǂݍ��݂����悤�ɂȂ�܂��B
  - �R���|�[�l���g�ł͂Ȃ��A�R���|�[�l���g���擾���邽�߂�Deferred����������܂��B

��:

    define(["container"], function(ContainerJS){
        /**
         * @class
         */
        var Class = function() { 
            this.a = ContainerJS.Inject;
            this.b = ContainerJS.Inject("foo.var");
            this.c = ContainerJS.Inject.all;
            this.d = ContainerJS.Inject.all("foo.var");
            this.e = ContainerJS.Inject.lazily;
            this.f = ContainerJS.Inject.all.lazily("foo.bal");
        };
        Class.prototype.method1 = {
            this.e.then( function(component) {
                // 
            }, function(error) {
                // 
            } )
        };
        return Class;
    });

�R���|�[�l���g��`���ɒ������邱�Ƃ��ł��܂��B

    var container = new ContainerJS.Container( function( binder ){
        binder.bind("Foo").withProperties({
            a : ContainerJS.Inject("foo.var")
        }).withConstructorArgument({
            b : ContainerJS.Inject.all.lazily("foo.bal")
        });
    });

## Initialization and Destruction

�R���|�[�l���g�쐬���ɌĂ΂��֐�(�������֐�)�Ɣj�����ɌĂ΂��֐�(�j���֐�)��o�^�ł��܂��B

- �������֐��́A�R���|�[�l���g�̍쐬/�ˑ�������/�C���^�[�Z�v�^�̓K�p�����ׂĊ���������Ɏ��s����܂��B
- �j���֐���container.Container#destroy()�����s�����ۂɈȉ��̏����𖞂����ꍇ�A���s����܂��B
  - �R���|�[�l���g�̃X�R�[�v��Singleton�܂���EagerSingleton�ł��邱�ƁB
  - �R���|�[�l���g��destroy()���s���ɍ쐬�ς݂ł��邱�ƁB
- �������֐��E�j���֐��́A�R���|�[�l���g�̃��\�b�h���Ŏw�肷����@�Ɗ֐��Ŏw�肷����@������܂��B
  - �֐����w�肵���ꍇ�A�����Ƃ��ăR���|�[�l���g�ƃR���e�i���n����܂��B

��:

    var c = new ContainerJS.Container( function( binder ) {
        
        // ���\�b�h���Ŏw��
        binder.bind( "Foo" ).onInitialize("initialize").onDestroy("dispose");

        // �֐��Ŏw��
        binder.bind( "Bar" ).onInitialize( function( component, container ) {
            component.initialize();
        }).onDestroy( function( component, container ) {
            component.dispose();
        });
    
    });

### Method Interception

�R���|�[�l���g�̃��\�b�h�ɃC���^�[�Z�v�^���������߂܂��B

- �C���^�[�Z�v�^�͊֐��Ŏw�肵�܂��B�����Ń��\�b�h����������i�[�����I�u�W�F�N�g���n����܂��B
- ��2�����ŁA�K�p����R���|�[�l���g����у��\�b�h�������֐����w��ł��܂��B
  - ��2��������������Ȃ��ꍇ�A���ׂăR���|�[�l���g�̂��ׂẴ��\�b�h�ɓK�p����܂��B

��:

        var container = new ContainerJS.Container( function( binder ){
            
            binder.bind("app.Component");
            
            binder.bindInterceptor( function( jointpoint ) {
               jointpoint.methodName;
               jointpoint.self;
               jointpoint.arguments; // �����B���ςł��܂��B
               jointpoint.context; // ���̃��\�b�h�Ăяo���̊Ԃŋ��L������Ԃ��i�[�ł��܂��B
               return jointpoint.proceed(); // �I���W�i���̃��\�b�h���Ăяo�����ʂ�Ԃ��܂��B
            }, function(binding, component, methodName) {
                if  (binding.name !== "app.Component" ) return false;
                return methodName === "method1"
                       || methodName === "method2";
            } );
        });
