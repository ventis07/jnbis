# Install #

Download the latest version of <a href='http://code.google.com/p/jnbis/downloads/list'>JNBIS</a> and add it to you project libraries.

# How To Use WSQ Decoder #

First of all you need to create an instance of _org.jnbis.WSqDecoder_.
```
    WsqDecoder wsqDecoder = new WsqDecoder();
```

Now you can decode your WSQ image using following methods:

  * Decode a WSQ image by passing image file name.
```
    public Bitmap decode(String fileName) throws IOException;
```
  * Decode a WSQ image by passing image File (java.io.File).
```
    public Bitmap decode(File file) throws IOException;
```
  * Decode a WSQ image by passing image stream.
```
    public Bitmap decode(InputStream inputStream) throws IOException;
```
  * Decode a WSQ image by passing image byte array.
```
    public Bitmap decode(final byte[] data);
```

The result of all methods is a Bitmap object which contains the decoded data in byte array format. you can convert it to other image formats using _org.jnbis.ImageUtils_ class.

**Example: convert to Jpeg**
```
        WsqDecoder wsqDecoder = new WsqDecoder();
        ImageUtils imageUtils = new ImageUtils();   
     
        String myfile = "/path/to/my/wsq image";

        Bitmap bm = wsqDecode.docode(myfile);
        byte[] data = imageUtils.bitmap2jpeg(bm);

        FileOutputStream bos = new FileOutputStream("myjpeg.jpg");
        bos.write(data);
        bos.close();
```



# How To Use NIST Decoder #

> Just like WsqDecoder, you need to create an instance of org.jnbis.nist.NistDecoder.
```
    NistDecoder nistDecoder = new NistDecoder();
```

Now you can decode your NIST file using following methods:

  * Decode a NIST file by passing file name.
```
    public DecodedData decode(String fileName, DecodedData.Format fingerImageFormat) throws IOException;
```
  * Decode a NIST file by passing File (java.io.File).
```
    public DecodedData decode(File file, DecodedData.Format fingerImageFormat) throws IOException;
```
  * Decode a NIST file by passing stream.
```
    public DecodedData> decode(InputStream inputStream, DecodedData.Format fingerImageFormat) throws IOException;
```
  * Decode a NIST file by passing byte array.
```
    public DecodedData decode(final byte[] data, DecodedData.Format fingerImageFormat);
```

  * Second parameter in above methods is the output image format and currently JPEG, GIF and PNG are supported.

  * DecodedData contains two type of data
  1. Text Data:
> > A map (key, value) of all decoded text information.
```
    // Returns all decoded keys.
    public Set<Integer> getTextKeys();

    // Return decoded string (Text) info, for given key.
    public String getText(Integer key);

```
  1. Binary Data:
> > A map (key, value) of all decoded images.
```
    // Returns all decoded keys
    public Set<Integer> getBinaryKeys();

    // Returns decoded image for given key.
    public BinaryData getBinary(Integer key);
```

  * key or index 0 is used to store decoded **custom** image (usually person face)
  * key or index 1..14 are used to store decoded **finger** images.

  * Sample code :
```
            DecodedData decoded = nistDecoder.decode(file, DecodedData.Format.GIF);

            Set binaryKeys = decoded.getBinaryKeys();

            for (Integer key : binaryKeys) {
   
                DecodedData.BinaryData bd = decoded.getBinary(key);
                
                FileOutputStream bos = new FileOutputStream(file + "-" + key + "." + bd.getType());
                bos.write(bd.getData());
                bos.close();
            }

```