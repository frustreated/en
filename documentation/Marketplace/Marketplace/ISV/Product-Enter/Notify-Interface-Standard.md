# 1 Introduction
This document describes the interface definition that the third-party applications need to implement to access to JD Cloud Marketplace. After the customer purchases goods in JD Cloud Marketplace, JD Cloud Marketplace will send the information of the goods and account information of the buyer to the service provider by calling the interface provided in this document. The service provider can obtain the information of ordering, renewal, expiration and discontinuation. At the same time, the service provider processes the buyer information according to the information. All interfaces involved in the document are called in HTTP GET and the message is transmitted in JSON format.
# 2 Scope
Software applications generally have independently accessible website. Customers do not need to purchase independent cloud resources, and can register, log in, and operation functions on the website through the Internet. Selection of commodity access type application software:
![image](https://github.com/jdcloudcom/cn/blob/edit/documentation/Marketplace/Marketplace/MarketPlace-Image/%E9%80%9A%E7%9F%A5%E6%8E%A5%E5%8F%A31.png)
# 3 Security
3.1 Description
To ensure the communication security between JD Cloud Marketplace and third-party applications, the Marketplace will add a token as a Security Token each time it calls the application interface. After receiving the request, the application interface generates a token value according to the generation rule and compares it with the token value obtained in the interface parameter, and it will pass the validation if the value is the same. The token value is composed of the parameters passed each time and the key assigned by the Marketplace to the service provider.
3.2 Key value acquisition
Obtained from the Service Management Backend (http://i-market.jdcloud.com/) Information Management > Basic Information page.

![image](https://github.com/jdcloudcom/cn/blob/edit/documentation/Marketplace/Marketplace/MarketPlace-Image/%E9%80%9A%E7%9F%A5%E6%8E%A5%E5%8F%A32.png)
3.3 Token value generation
3.3.1 Description
The token value is the security check parameter between the Marketplace and the service provider, and the Marketplace will have a token value in every parameter each time it calls the provider interface. The service provider generates a token value according to the generation rule and compares it with the token value obtained in the interface. It will pass the validation if the value is the same.

 3.3.2 Generation rules

Take all the parameters except the token in each http get request parameter, perform dictionary sorting on the parameter name, add &key=[isv key value] at the end of the sorted character string, and then encrypt the entire character string by md5 encryption The encrypted string is used as the token value.

3.3.3 Example
l Example of a request received by a service provider:
http://www.isvwebsite.com?p1=1&p2=2&p3=3&token=xxxx                

2 Sorting operation: no url encode
sort（P1，P2，P3）；                

3 Token value: URLEncoder.encode (character string after splicing, "utf-8");
"p1=1&p2=2&p3=3&key=isvkey".toMD5()

The document has the instance code at the end.

Test key:

qweqeqeqe123123123131

 

Test parameter:

accountNum=1&action=createInstance&email=bujiaban%40jd.com&expiredOn=2018-06-30+23%3A59%3A59&jdPin=bujiaban&mobile=&orderBizId=444181&orderId=556596&serviceCode=FW_GOODS-500232&skuId=FW_GOODS-500232-1&template=&token=9512df22a941f172a9f28068b758ee3e
# 4 Interface description
l If the customer purchases the product and the payment is successful, the Marketplace will call the interface to notify the service provider of the purchase information and customer information.

2 The service provider needs to return the delivery instance unique ID (instanceId) corresponding to this order. It is recommended that this ID directly use the orderBizId passed in the Marketplace.

3 Please do not block this interface. If it takes a long time, you can use the queue to buffer, set instanceId=0, and then return immediately. If the operation fails, please set instanceId=0, the Marketplace will call again until a valid instanceId is obtained. If the valid instanceId has not been obtained, the Marketplace will continue to call 200 times and then stop calling. If the service provider interface problem has been resolved, you need to request the call again. Please log in to the service provider management backend (http://i-market. Jcloud.com/)--Transaction Management—Click "Reset Instance" in the order record query page. When instanceId=0 or exception, the interface may be called multiple times. If it is called repeatedly, please ensure the idempotency of the interface.

Note: Orders generated by the service sold by quantity will call the interface multiple times according to the purchase quantity. The orderBizId will be different. Please return a delivery instance for each call.
4.1 New purchases
4.1.1 Description
l After the user places an order and pays successfully, the Marketplace calls the new service interface, and the user information and order information are transmitted. The service provider performs the delivery action in its system according to the parameters passed in, and returns the instance information to the Marketplace through the Json format. After the Marketplace receives the return value, it saves the information and feeds it back to the user. The interface call process is as follows:
![image](https://github.com/jdcloudcom/cn/blob/edit/documentation/Marketplace/Marketplace/MarketPlace-Image/%E9%80%9A%E7%9F%A5%E6%8E%A5%E5%8F%A33.jpg)
1. The return value of the service provider needs to contain the unique ID (instanceId) of the instance created by this order. It is recommended that this ID directly use the orderBizId passed in the Marketplace.

2. Please do not block this interface. If it takes a long time, you can use the queue to buffer, set instanceId=0, and then return immediately. If the operation fails, please set instanceId=0, the Marketplace will call again until a valid instanceId is obtained.

3. The interface may be called multiple times. If it is called repeatedly, please ensure the idempotency of the interface.

4.1.2 Request parameters
![image](https://github.com/jdcloudcom/cn/blob/edit/documentation/Marketplace/Marketplace/MarketPlace-Image/%E9%80%9A%E7%9F%A5%E6%8E%A5%E5%8F%A34.png)
4.1.3 Return parameters
![image](https://github.com/jdcloudcom/cn/blob/edit/documentation/Marketplace/Marketplace/MarketPlace-Image/%E9%80%9A%E7%9F%A5%E6%8E%A5%E5%8F%A35.png)
Description: The appInfo field contains the information required after the customer purchases the product, logs in to the service address (website) for subsequent operations, or implements the client's login-free access (requires return to the free login website).

The Info field is an operation to customize the key-value field when appinfo can't meet the service provider's special login requirements to provide the customer with the new purchase.

4.1.4 Example
Request:
http://www.isvwebsite.com?action=createInstance&email=&expiredOn=2017-01-08+00%3A00%3A00&jdPin=test_jdb22&mobile=&orderBizId=423499&orderId=519801&serviceCode=FW_GOODS-409717&skuId=FW_GOODS-409717-1&template=&token=7e8970385f7f263074a48852aeda12a5&extraInfo={"key1":"1","key1","2"}
Return:
```
{

       "instanceId":   "1001",

       "appInfo": {

       "frontEndUrl":   "http://xxx.com/",

       "adminUrl":   "http://xxx.com/admin",

       "username":   "admin",

       "password":   "admin_password"

       …

},

"info": {

       "key1": "Customized information"

       }

}
```
4.2 Renew:
4.2.1 Description

After the user renews and pays successfully, the Marketplace calls the renewal interface, passing in the instance ID and the new expiration date. The service provider updates the due date maintained in its system and returns a successful identification. After the Marketplace receives the return value, it processes the service instance accordingly. The interface call process is as follows:
![image](https://github.com/jdcloudcom/cn/blob/edit/documentation/Marketplace/Marketplace/MarketPlace-Image/%E9%80%9A%E7%9F%A5%E6%8E%A5%E5%8F%A36.jpg)

4.2.2 Request parameters
![image](https://github.com/jdcloudcom/cn/blob/edit/documentation/Marketplace/Marketplace/MarketPlace-Image/%E9%80%9A%E7%9F%A5%E6%8E%A5%E5%8F%A37.jpg)
4.2.3 Return parameters
![image](https://github.com/jdcloudcom/cn/blob/edit/documentation/Marketplace/Marketplace/MarketPlace-Image/%E9%80%9A%E7%9F%A5%E6%8E%A5%E5%8F%A38.jpg)
4.2.4 Example
l Request
http://www.isvwebsite.com? action=renewInstance&expiredOn=2017-12-06+00%3A00%3A00&instanceId=1002&orderId=520801&token=475f28682b5d0d1af820ffd477c1188f
2 Return
`{‘success’:true’, ‘authCode:’123456789’, ‘message’:’renew suecess’}` 

4.3 Release
4.3.1 Description
The Marketplace will call the interface after the order is cancelled and the refund is successful. The service provider can delete the specified instance in the implementation of the interface. The interface call process is as follows:
![image](https://github.com/jdcloudcom/cn/blob/edit/documentation/Marketplace/Marketplace/MarketPlace-Image/%E9%80%9A%E7%9F%A5%E6%8E%A5%E5%8F%A39.jpg)
4.3.2 Request parameters
![image](https://github.com/jdcloudcom/cn/blob/edit/documentation/Marketplace/Marketplace/MarketPlace-Image/%E9%80%9A%E7%9F%A5%E6%8E%A5%E5%8F%A39.png)
4.3.3 Return parameters
![image](https://github.com/jdcloudcom/cn/blob/edit/documentation/Marketplace/Marketplace/MarketPlace-Image/%E9%80%9A%E7%9F%A5%E6%8E%A5%E5%8F%A310.png)
4.3.4 Example
l Request

http://www.isvwebsite.com?action=releaseInstance&instanceId=424499_520001_FW_GOODS-409717-1&token=cb0a8e6348de9d12e74a085a28bf00fe
2 Return
`{‘success’: true,’message’:’release success’} `               

4.4 Free login
4.4.1 Description

After the customer purchases the product, he can log in to the service provider system through the free login interface. The Marketplace will organize a url for free login based on the authUrl in appInfo returned by the "new purchase commodity" interface and the following parameters. ISV will verify whether the token is legal after receiving the request of this url and confirm whether timeStamp complies with ISV constraint time, so as to achieve automatic login after ISV management.

4.4.2 Request parameters
![image](https://github.com/jdcloudcom/cn/blob/edit/documentation/Marketplace/Marketplace/MarketPlace-Image/%E4%B8%BB%E5%8A%A8%E9%80%9A%E7%9F%A511.png)

4.4.3 Example
l Request
http://www.isvwebsite.com?token=9560d4d52cab35689fd5d472f28119ab&action=verify&instanceId=1001&timeStamp=2016-12-01+10%3A30%3A01              

2 Return
Login success page

4.5 Upgrade
4.5.1 Description

After the user upgrades and pays successfully, the Marketplace calls the upgrade interface and passes in the instance ID and the new revision. The service provider updates the revision maintained in its system and returns a successful identification. After the Marketplace receives the return value, it processes the service instance accordingly. The interface call process is as follows:
![image](https://github.com/jdcloudcom/cn/blob/edit/documentation/Marketplace/Marketplace/MarketPlace-Image/%E9%80%9A%E7%9F%A5%E6%8E%A5%E5%8F%A312.jpg)


4.5.2 Request parameters
![image](https://github.com/jdcloudcom/cn/blob/edit/documentation/Marketplace/Marketplace/MarketPlace-Image/%E9%80%9A%E7%9F%A5%E6%8E%A5%E5%8F%A313.png)
Description: extraInfo: Json format key-value dynamic field, used to transfer some non-general fields to the service provider after the user places an order, such as the specification upgrade of some products to 20G;
4.5.3 Return parameters
![image](https://github.com/jdcloudcom/cn/blob/edit/documentation/Marketplace/Marketplace/MarketPlace-Image/%E9%80%9A%E7%9F%A5%E6%8E%A5%E5%8F%A314.png)
4.5.4 Example
l Request
http://www.isvwebsite.com?action=upgradeInstance&skuId=FW-123-1instanceId=1002&orderId=520801&token=475f28682b5d0d1af820ffd477c1188f&extraInfo={"key1":"1","key1","2"} 

2 Return
`{‘success’:true’, ‘authCode:’123456789’, ‘message’:’renew suecess’}`

4.6 Expansion
4.6.1 Description

After the user expands and pays successfully, the Marketplace calls the expansion interface and passes in the instance ID and the new revision. The service provider updates the revision maintained in its system and returns a successful identification. After the Marketplace receives the return value, it processes the service instance accordingly. The interface call process is as follows:
![image](https://github.com/jdcloudcom/cn/blob/edit/documentation/Marketplace/Marketplace/MarketPlace-Image/%E9%80%9A%E7%9F%A5%E6%8E%A5%E5%8F%A315.jpg)
4.6.2 Request parameters
![image](https://github.com/jdcloudcom/cn/blob/edit/documentation/Marketplace/Marketplace/MarketPlace-Image/%E9%80%9A%E7%9F%A5%E6%8E%A5%E5%8F%A316.png)
Description: extraInfo: Json format key-value dynamic field, used to pass some non-general fields to the service provider after the user places an order, for example, the specification of some product expansion is 20G; the number of user expansion accounts: 5;

4.6.3 Return parameters
![image](https://github.com/jdcloudcom/cn/blob/edit/documentation/Marketplace/Marketplace/MarketPlace-Image/%E9%80%9A%E7%9F%A5%E6%8E%A5%E5%8F%A317.png)

4.6.4 Example
l Request
```
http://www.isvwebsite.com?action=dilateInstance&accountNum=1&instanceId=1002&orderId=520801&token=475f28682b5d0d1af820ffd477c1188f&extraInfo={"key1":"1","key1","2"}
```

2 Return
`{‘success’:true’, ‘authCode:’123456789’, ‘message’:’dilatesuccess’}`

4.7 Expiration
4.7.1 Description

The Marketplace will call the interface after the subscription item expires, and the service provider can set the specified instance as expired in the implementation of the interface. The interface call process is as follows:
![image](https://github.com/jdcloudcom/cn/blob/edit/documentation/Marketplace/Marketplace/MarketPlace-Image/%E9%80%9A%E7%9F%A5%E6%8E%A5%E5%8F%A318.png)
4.7.2 Request parameters
![image](https://github.com/jdcloudcom/cn/blob/edit/documentation/Marketplace/Marketplace/MarketPlace-Image/%E9%80%9A%E7%9F%A5%E6%8E%A5%E5%8F%A319.png)

4.7.3 Return parameters
![image](https://github.com/jdcloudcom/cn/blob/edit/documentation/Marketplace/Marketplace/MarketPlace-Image/%E9%80%9A%E7%9F%A5%E6%8E%A5%E5%8F%A320.png)

4.7.4 Example
l Request
http://www.isvwebsite.com? action=   expiredInstance &instanceId=424499_520001_FW_GOODS-409717-1&token=cb0a8e6348de9d12e74a085a28bf00fe

2 Return
`{‘success’: true,’message’:’ expiredInstance success’}`

5.1 Description

In order to ensure the correctness of the automatic opening of the software service, the application access debugging interface is built on the service provider management platform. The service provider can debug the correctness of the above interfaces on this page.

Note: the debugging data is informal, and JD Cloud Marketplace does not do business processing.
```
import java.io.UnsupportedEncodingException;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;

public class Md5Util {

    private static String byteArrayToHexString(byte b[]) {
        StringBuffer resultSb = new StringBuffer();
        for (int i = 0; i < b.length; i++) {
            resultSb.append(byteToHexString(b[i]));
        }

        return resultSb.toString();
    }

    private static String byteToHexString(byte b) {
        int n = b;
        if (n < 0)
            n += 256;
        int d1 = n / 16;
        int d2 = n % 16;
        return hexDigits[d1] + hexDigits[d2];
    }

    /**
     * The method for saas interface to acquire the token
     *
     * @param origin
     * @return
     */
    public static String md5Encode(String origin) {
        try {
            MessageDigest md = MessageDigest.getInstance("MD5");
            return byteArrayToHexString(md.digest(origin.getBytes("utf-8")));
        } catch (NoSuchAlgorithmException | UnsupportedEncodingException e) {
            e.printStackTrace();
        }

        return null;

    }
    private static final String hexDigits[] = {"0", "1", "2", "3", "4", "5",
            "6", "7", "8", "9", "a", "b", "c", "d", "e", "f"};

    public static void main(String[] args) {
        String md2 = Md5Util.md5Encode("accountNum=1&action=createInstance&email=bujiaban@jd.com&expi

```



















