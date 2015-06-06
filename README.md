#HTTP-Service
___

###What is this?
***
HTTPService is a class written in Objective-C that wraps functionality of AFHTTPSessionManager for all HTTP communication. It provide two methods, one that uses Raw JSON format and another that uses form-data format as HTTP encoding. You should either config `BASE_URL` in `HTTPService.h` or use designated initializer `- (id)initWithBaseURL:(NSURL*) baseURL andSessionConfig:(NSURLSessionConfiguration*) config`.

###How to use?
***

HTTPService is based on [**AFNetworking**](http://afnetworking.com). You need to add AFNetworking to your XCode project first - [Github/AFNetworking](https://github.com/AFNetworking/AFNetworking)

After adding AFNetworking, you need to add `HTTPService.h` and `HTTPService.m` to your XCode project. You can initialize HTTPService class object by two ways - 

**1. Default -**

```objective-c
HTTPService *httpService = [[HTTPService alloc] init];
```

**Note:** *if you choose default method for initializing then you should config `BASE_URL` in `HTTPService.h`.*

**2. Using designated initializer -**

```objective-c
NSString *baseURLString = @"http://example.com/mobile/";
NSURL *baseURL = [NSURL URLWithString:baseURLString];
NSURLSessionConfiguration *urlSession = [NSURLSessionConfiguration defaultSessionConfiguration];
HTTPService *httpService = [[HTTPService alloc] initWithBaseURL:baseURL andSessionConfig:urlSession];
```

Now, you can do HTTP request by using two available instance methods. You can choose one of them based on your HTTP encoding requirement - 

**1. Raw JSON encoding -**

```objective-c
NSMutableDictionary *headers = [[NSMutableDictionary alloc] initWithObjectsAndKeys:@"ab23tcdsjbvhd",@"token",nil];
NSMutableDictionary *params = [[NSMutableDictionary alloc] initWithObjectsAndKeys:@"value1",@"param1",@"value2",@"param2", nil];
[httpService startRequestWithHttpMethod:kHttpMethodTypePost
                            withHttpHeaders:headers
                            withServiceName:@"serviceName"
                             withParameters:params
                                withSuccess:^(NSURLSessionDataTask *task, id responseObject) {
                                    NSLog(@"response - %@",responseObject);
                                }
                                withFailure:^(NSURLSessionDataTask *task, NSError *error) {
                                    NSLog(@"error - %@",[error localizedDescription]);
                                }];
```

#####You need to pass appropriate HTTP Method to it from following - 

`GET` - *kHttpMethodTypeGet*

`POST` - *kHttpMethodTypePost*

`DELETE` - *kHttpMethodTypeDelete*

`PUT` - *kHttpMethodTypePut*


**Note:** *There is no need to pass **Content-Type** and **Accept** in header. It uses Content-Type "application/json"*

**2. Multipart form data -**

```objective-c
NSMutableDictionary *headers = [[NSMutableDictionary alloc] initWithObjectsAndKeys:@"ab23tcdsjbvhd",@"token",nil];
NSMutableDictionary *params = [[NSMutableDictionary alloc] initWithObjectsAndKeys:@"value1",@"param1",@"value2",@"param2", nil];
NSData *fileData = UIImageJPEGRepresentation(image, 1.0);
[httpService uploadFileRequestWithHttpHeaders:headers
                                  withServiceName:@"serviceName"
                                   withParameters:params
                                     withFileData:[NSArray arrayWithObject:fileData]
                                      withSuccess:^(NSURLSessionDataTask *task, id responseObject) {
                                          NSLog(@"response - %@",responseObject);
                                      }
                                      withFailure:^(NSURLSessionDataTask *task, NSError *error) {
                                          NSLog(@"error - %@",[error localizedDescription]);
                                      }];
```

**Note:** 

* *There is no need to pass **Content-Type** in header. It uses Content-Type "multipart/form-data"*

* *This methods takes array of NSData objects, so you can pass multiple files in it.*