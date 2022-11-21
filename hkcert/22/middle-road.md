# Middle Road (350 points, 8 solves)
# 中間真係有道？

By [`R1ckyH`](https://github.com/R1ckyH)

>是咁的，30秒男? 唔係啊，我做得耐過你啊！

>I hate java, recommend using `jvav`

Search [online decompiler](http://www.javadecompilers.com/apk) at `google` and decompile the `app.apk`
We can now find all things on `com.example package` and `decryptstringmanager` 

We find the `app` mainly uses `GetFlag.java` to get `flag`.
But most of the string is encrypted. So I use the method in `DecryptString.java` to decrypt the string.

After decrypting the string, we can see the app is using API to get the flag. The app will generate the AES key randomly and use RSA encryption to encrypt the AES key and send it to the server to get the encrypted flag. That means we can generate our own AES key to decrypt the encrypted flag.

Then, I download the Android apk and run it on my phone. I used BurpSuite to hijack the request and modified it to my key immediately.

>requests are like this
```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 1262

{"rsa_e":"65537","rsa_n":"630791816259958205673016318224240858751136661280471188438412165447232723393914969097252787347626988321917000774733737322494449818919125260958648800263996495910600551815631575048264209421462043584096144705567721409215513579478663560574161504773513061565180193507862861434089810710400523178487707581275521520577795243670305564926506609230306209563205662716453787880405129570055856427308236053212836347739247466071945057090182041305420394821265067125791719709125731551049817247167239327835167637907528587926920743330992051512069741151473403496874341619954964058090710784149252382410520680830775602521185581289313125400239124506053645292697781762946657933842988207613736173565401062037912720334806569630800708139852384275452749639040442985616234470227456393754010233488011575792824583072056560579111260482563707424996590173055927167750726725710388553693042649995924410781689294007749036207493130036597566481094607536687014042687135417672264358144088925155507117340622725123068145705074122591740835971852863145356002433770919362756255797144813740238108754874435077344162655866048790680086970052985926804608808427216687157188698375760724339352425399301906941294728294761952114211421984549491710888098698226680830192527065831968050134441967"}
```
```http
POST /getFlag HTTP/1.1
Host: middle-road.hkcert22.pwnable.hk:4433
Content-Type: application/x-www-form-urlencoded
Content-Length: 762
Accept-Encoding: gzip, deflate
User-Agent: okhttp/4.10.0
Connection: close

key=bWQnegekncKm2zshf8D5NW9JynInrYn3ugQi%2Fmp%2FZmhg54L%2FsDDO5mmkDLq%2B0ZQtnMHjG524gFPUm%2FaRkHwiqNxkfKomaNX%2B57R%2BCkmx9B3hly4OjU3xAx%2FCI4%2BCvDvZ2Grik%2FLVNEKgjMGS1GC9ekXOF1%2B0XjBV21%2B6Il%2Br%2BwDvbhuemeq4YZHhYS4cnbEN9P01rb6BKbw7HT%2BPPRiElcDBhDXgeWFbiiX5BuIH1Ys91GgQYEqLapsXgjI2SAeuAmBbPdPWtuVlj0DDIJG%2Fa3dxI77DV26CtNgwgMtxcok%2BOQYCADyJz%2BSfeUxRDcAaJcqhxeZxtR5YV%2BKYb7l%2F0nX5dcrraagcP7USNchhnDN4efDEgZngCS2T73SEPvYjsiQhxsZQXG0CHeLGT6k%2FCxERZS96yoNzE4UZbsLhIXtxWFHmZ8aUXRs2harlEzr5h5%2F%2B0TLOHum5KVYG2B8hOKIgnBLwX5gjHdWwhuJFmGFgKGswr%2F9x3OmAtF3IAbuUCmSkkb6HvfAA%2Fz1V61JkEWR2fcsYMKHaiU8Ar%2BXDvwjO60v7vs5bCfkZz4Hqnac77UujR3xjCX5CR5fkZHKSBsR6PIbbNP8do0eEgh881TAJep3q4OtD5LviArZflnzizc%2BvB9Za0mz4bRTxIPrP8lNCI5a1luo%2FTuP%2B91EpigI%3D&code=44748332
```

Now the OTP code exists, 
The encrypted flag now exists.
We just need to make our own `AES-256` bit key to decrypt the `flag-256` bit.


> This [website](https://www.allkeysgenerator.com/Random/Security-Encryption-Key-Generator.aspx) can gen ase-256 key

we just need to encrypt this key with RSA, I copy some code from method `m47c` and from [stackoverflow](https://stackoverflow.com/questions/140131/convert-a-string-representation-of-a-hex-dump-to-a-byte-array-using-java) to make the encryption, and I use this script to generate the `base64` and then throw into [this website](https://www.urlencoder.org/) to `URL-encode` format
> code modify from [web](https://www.urlencoder.org/) and `GetFlag.java` by [`R1ckyH`](https://github.com/R1ckyH)
```java
import com.decryptstringmanager.DecryptString;
import java.math.BigInteger;
import java.security.KeyFactory;
import java.security.spec.RSAPublicKeySpec;
import java.util.Base64;
import javax.crypto.Cipher;
public class test {
    public static byte[] hexStringToByteArray(String s) {
        int len = s.length();
        byte[] data = new byte[len / 2];
        for (int i = 0; i < len; i += 2) {
            data[i / 2] = (byte) ((Character.digit(s.charAt(i), 16) << 4)
                                + Character.digit(s.charAt(i+1), 16));
        }
        return data;
    }

    public static void main(String[] args) {
        BigInteger bigInteger = new BigInteger("630791816259958205673016318224240858751136661280471188438412165447232723393914969097252787347626988321917000774733737322494449818919125260958648800263996495910600551815631575048264209421462043584096144705567721409215513579478663560574161504773513061565180193507862861434089810710400523178487707581275521520577795243670305564926506609230306209563205662716453787880405129570055856427308236053212836347739247466071945057090182041305420394821265067125791719709125731551049817247167239327835167637907528587926920743330992051512069741151473403496874341619954964058090710784149252382410520680830775602521185581289313125400239124506053645292697781762946657933842988207613736173565401062037912720334806569630800708139852384275452749639040442985616234470227456393754010233488011575792824583072056560579111260482563707424996590173055927167750726725710388553693042649995924410781689294007749036207493130036597566481094607536687014042687135417672264358144088925155507117340622725123068145705074122591740835971852863145356002433770919362756255797144813740238108754874435077344162655866048790680086970052985926804608808427216687157188698375760724339352425399301906941294728294761952114211421984549491710888098698226680830192527065831968050134441967");
        BigInteger bigInteger2 = new BigInteger("65537");
        try{
            Cipher instance = Cipher.getInstance("RSA/ECB/NoPadding");
            instance.init(1, KeyFactory.getInstance("RSA").generatePublic(new RSAPublicKeySpec(bigInteger, bigInteger2)));
            System.out.println(Base64.getEncoder().encodeToString(instance.doFinal(hexStringToByteArray("413F4428472B4B6250655368566D5971337436773979244226452948404D6351"))));
        }catch(Exception e){

        }
    }
}

```
>我是30秒的男人

So we just need to copy the `URL-encode` key and put it into the `post request`, and send it out fast, so I use [`burp suite`](https://portswigger.net/burp) to send through the repeater.

```http
POST /getFlag HTTP/1.1
Host: middle-road.hkcert22.pwnable.hk:4433
Content-Type: application/x-www-form-urlencoded
Content-Length: 762
Accept-Encoding: gzip, deflate
User-Agent: okhttp/4.10.0
Connection: close

key=bWQnegekncKm2zshf8D5NW9JynInrYn3ugQi%2Fmp%2FZmhg54L%2FsDDO5mmkDLq%2B0ZQtnMHjG524gFPUm%2FaRkHwiqNxkfKomaNX%2B57R%2BCkmx9B3hly4OjU3xAx%2FCI4%2BCvDvZ2Grik%2FLVNEKgjMGS1GC9ekXOF1%2B0XjBV21%2B6Il%2Br%2BwDvbhuemeq4YZHhYS4cnbEN9P01rb6BKbw7HT%2BPPRiElcDBhDXgeWFbiiX5BuIH1Ys91GgQYEqLapsXgjI2SAeuAmBbPdPWtuVlj0DDIJG%2Fa3dxI77DV26CtNgwgMtxcok%2BOQYCADyJz%2BSfeUxRDcAaJcqhxeZxtR5YV%2BKYb7l%2F0nX5dcrraagcP7USNchhnDN4efDEgZngCS2T73SEPvYjsiQhxsZQXG0CHeLGT6k%2FCxERZS96yoNzE4UZbsLhIXtxWFHmZ8aUXRs2harlEzr5h5%2F%2B0TLOHum5KVYG2B8hOKIgnBLwX5gjHdWwhuJFmGFgKGswr%2F9x3OmAtF3IAbuUCmSkkb6HvfAA%2Fz1V61JkEWR2fcsYMKHaiU8Ar%2BXDvwjO60v7vs5bCfkZz4Hqnac77UujR3xjCX5CR5fkZHKSBsR6PIbbNP8do0eEgh881TAJep3q4OtD5LviArZflnzizc%2BvB9Za0mz4bRTxIPrP8lNCI5a1luo%2FTuP%2B91EpigI%3D&code=44748332
```
```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 164

{"aes_cbc_iv":"+hsxVFVlYFTCMfRroXnSBA==","enc_flag":"TKd4R+A2tT8cy0LNjFc3QeFBuCiLzLz9iHkrrnjv8vbmeWzi/3baSIUrBhTG3t6MxgVUzUAwsXctJUyS9pZqc6fsXQAovKAdZXvUhECOodE="}
```

I just copy the result to [CyberChef](https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true,false)AES_Decrypt(%7B'option':'Hex','string':'413F4428472B4B6250655368566D5971337436773979244226452948404D6351'%7D,%7B'option':'Base64','string':'%2BhsxVFVlYFTCMfRroXnSBA%3D%3D'%7D,'CBC','Raw','Raw',%7B'option':'Hex','string':''%7D,%7B'option':'Hex','string':''%7D)&input=VEtkNFIrQTJ0VDhjeTBMTmpGYzNRZUZCdUNpTHpMejlpSGtycm5qdjh2Ym1lV3ppLzNiYVNJVXJCaFRHM3Q2TXhnVlV6VUF3c1hjdEpVeVM5cFpxYzZmc1hRQW92S0FkWlh2VWhFQ09vZEU9) and then get the flag:


>`hkcert22{CLi3NT_can_B3_reverSE_EnGIN33red_by_0ne_W4y_or_aNoTh3r}`
