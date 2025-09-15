# Popcat
We were given an APK file `popcat.apk`. When installed, the app looked like a simple Popcat clicker — every tap increased the counter.  
The challenge statement hinted that the flag was hidden somewhere inside the application logic, not directly shown in the UI.

<img width="795" height="279" alt="Screenshot 2025-09-14 124536" src="https://github.com/user-attachments/assets/4d729988-433b-4fb7-9ed7-884ff91bb898" />

### Challenge
<img width="365" height="662" alt="Screenshot 2025-09-14 124605" src="https://github.com/user-attachments/assets/ec0215f5-8a65-4813-8656-fa6bc9a27bef" />

I started by opening the APK in **JADX** to decompile the code.  
Inside the package `com.girlsinctf.popcat`, I found a suspicious class called **`FlagProvider`**.

The important source code:

```java
public final class FlagProvider {
    public static final FlagProvider INSTANCE = new FlagProvider();

    public final String getFlag(Context context) {
        String f1 = context.getString(R.string.f1);
        String f2 = context.getString(R.string.f2);

        byte[] k1 = "meowmeowmeowmeow".getBytes(Charsets.UTF_8);
        byte[] k2 = "meowmeowmeowmeowmeow".getBytes(Charsets.UTF_8);

        byte[] c1 = Base64.decode(f1, 0);
        byte[] c2 = Base64.decode(f2, 0);

        // XOR with key1 then reverse
        byte[] bArr = new byte[c1.length];
        for (int i = 0; i < c1.length; i++) {
            bArr[i] = (byte) (c1[i] ^ k1[i % k1.length]);
        }
        byte[] h1 = ArraysKt.reversedArray(bArr);

        // XOR with key2 then reverse
        byte[] bArr2 = new byte[c2.length];
        for (int i2 = 0; i2 < c2.length; i2++) {
            bArr2[i2] = (byte) (c2[i2] ^ k2[i2 % k2.length]);
        }
        byte[] h2 = ArraysKt.reversedArray(bArr2);

        // Final flag
        return new String(ArraysKt.plus(h1, h2), Charsets.UTF_8);
    }
}
```
From this, we learn:

- The flag is not directly visible but is generated at runtime.

- It uses two encoded strings (f1, f2) from the app’s resources (strings.xml).

- Both values are Base64 decoded, XOR’d with repeating keys (meow…), reversed, then concatenated
Extracted resource values:
```
<string name="f1">MlUbKF4IXxQBVhgMWFcpIy4i</string>
<string name="f2">EEROVkwiAUYGBls/MgEGQz8BAUM</string>
```


