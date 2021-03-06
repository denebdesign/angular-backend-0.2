# Angular Backend

Angular backend server api version 0.2



# Installation

````
$ git clone https://github.com/thruthesky/angular-backend-0.2
````




# How To Use (사용법)

먼저 angular-backend-0.2 폴더에 있는 모듈을 사용 할 수 있도록 적절하게 import 한다.

app.module.ts)

````
	import { AngularBackendModule } from './angular-backend-0.2/angular-backend.module';

	imports: [
		AngularBackendModule
	],
````

테스트를 하기 위해서는 아래와 같이 한다.

app.component.ts)

````
	import { Test } from './angular-backend-0.2/test';
	constructor( test: Test ) {}
````

# Utilities

## getErrorString( error )

API 호출에서 에러가 발생한 경우, 그 에러를 파라메타로 전달하면 자세한 에러 내용을 문자열로 리턴한다.


# API

## successCall()

successCall() 은 결과가 항상 성공(OK)이다. 결과 값은 version 값을 리턴한다. 테스트 용도로 사용 할 수 있다.

````
this.backend.successCall().subscribe( re => {
    console.log("Success: Version: " + re['data']['version']);
}, err => console.log("Error: ", err ) );
````


## errorCall()

errorCall() 은 항상 실패의 값을 받는다. 테스트 용도로 사용할 수 있다.

````
this.backend.errorCall().subscribe( re => {
    this.error(re, 'This should be an error. But success ' + this.backend.getErrorString( re ));
}, ( error ) => {
    this.success("errorCall() : This is fake error. " + this.backend.getErrorString(error) );
    } );
````


## scriptError()

서버에서 PHP 에러 메세지를 전달한다. 테스트 용도로 사용할 수 있다.

````
this.backend.scriptError().subscribe( re => {
    console.log(re);
    this.error( re, "scriptError() - This should be script error. But success." );
}, error => {
    console.log( error );
    this.success( 'This should be script error. This is PHP script error.' );
});
````



## timeoutError()

PHP 에서 sleep(50) 를 호출하여, timeout 을 발생 시킨다.

````
this.backend.timeoutError().subscribe( re => {
    console.error( re, "This should be timeout error. But success." );
}, error => {
    if ( error.message == ERROR_TIMEOUT ) {
        this.success('This should be timeout error. ' + this.backend.getErrorString( error ));
    }
    else this.error( error, "This is not timeout error. But another error");
});
````
    

## internalServerError()

서버에서 500 에러를 전달한다. 테스트 용도로 사용 할 수 있다.

````
this.backend.internalServerError().subscribe( re => {
    this.error("This must be 500 internal server error. but success");
}, error => {
    console.log(error);
    if ( this.backend.isInternalServerError( error ) ) this.success("Internal Server Error: " + this.backend.getErrorString(error) );
    else this.error(error, "This must be 500 - internal server error. but it is another error.");
});
````


