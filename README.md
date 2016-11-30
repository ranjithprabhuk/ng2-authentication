# Ng2-Authentication
Ng2-authentication is a module to handle authentication in angular 2 front end using auth guard.


#
*Note:* this solutions is not included in npm, so download and include it in your service folder.

## Installation

```
	1. Download and include the authentication folder in your application.
	2. Inject the AuthenticationModule in your main AppModule
```

## Usage

Authentication Module is based on auth guard implementation in angular 2. Also access token is stored in the localstorage as auth_token.

```

import { Injectable } from '@angular/core';
import { ApiService } from '../../services/api.service'; //required to download and add this api service. It is available in my repository as ng2-rest-api

import { Observable } from 'rxjs/Observable';
import 'rxjs/add/observable/of';
import 'rxjs/add/operator/do';
import 'rxjs/add/operator/delay';

@Injectable()
export class AuthenticationService {

    protected authEndPoint: string = "authentication/signIn.json"; //authentication api end point need to be defined here. base url defined in api.service

    constructor(private apiService: ApiService) { }

    //to check the login credentials
    login(parameter?: any): Promise<any> {
        return this.apiService.get(this.authEndPoint)
            .then(res => {
                if (res) {
                    localStorage.setItem('auth_token', res.auth_token);
                    return res;
                }
            }).catch(err => err);
    }

    //to logout from the system
    logout(): void {
        localStorage.removeItem('auth_token');
    }

    //to check the login status
    checkLogin(): boolean {
        return !!localStorage.getItem('auth_token');
    }
}
```

### Using it in your component

1. Method for SignIn
	``` ts
		public signIn(data: any): void {
			if (data && data.username && data.password) {
				//call the authService to check the credentials
				this.authService.login(data).then(res => {
					console.log("response>>", res);
					this.router.navigate(['/dashboard']);
				}).catch(err => err);
			}
		}
	```
	
2. Method to Logout 
	``` ts
		public logout(): void {
			this.authService.logout();
		}
	```
	
3. Method to Check the Login Status on page load and navigation
	``` ts
		public checkLogin(): void {
			let status = this.authService.checkLogin(); // will return true if use already logged in
		}
		
		
	```


## About Author
* [Author URL] (http://ranjithprabhu.in)

I am passionate in playing with pixels, creating attractive designs which interact well with the user and love developing web apps. Have a good background in web design and development. Also having wonderful working experience with various interesting projects and participated in the development of the products to provide end to end solutions.


## License
Released under the MIT license.
