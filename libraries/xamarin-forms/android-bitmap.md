# Managing Bitmap Memory in Android (feat. Xamarin.Forms)

## Android Version Overview to support Bitmap
- Android 2.3.3(API 10) and backwards : the bitmap pixel data (native heap) is sotred in native memory which is separated from the bitmap(Dalvik heap) itself.
- Android 3.0(API 11) - Android 7.1(API 25) : the pixel data stored on the Dalvik heap along with the associated bitmap.
- Android 8.0 and onwards : the pixel data stored in the native heap

## Manage Memory
### Memory of Image Overview
- Bitmap to Memory by BitmapFactory
  ```
  Bitmap bitmap = BitmapFactory.decodeResource(getResources(), R.mipmap.hqimage);
  imageView.setImageBitmap(bitmap);
  ```
- Size of bitmap by ```bitmpa.getByteCount()```
  > 8 - 12time bigger than disk size of file on memory (res x res x 8bits~12bits)
- Recycle or Reuse bitmap memory is important

(* ref: https://medium.com/android-news/loading-large-bitmaps-efficiently-in-android-66826cd4ad53)

### Android 2.3.3(API 10) and backwards
- use ```recycle()```
- code snippet of recycle : see https://developer.android.com/topic/performance/graphics/manage-memory#java

### Android 3.0(API 11)
- save a bitmap for later use
- option 1) use ```BitmapFactory.Options.inBitmap```
  > only equal sized bitmaps are supported before Android 4.4 (API 19)
  ```
  public static Bitmap decodeSampledBitmapFromFile(String filename,
        int reqWidth, int reqHeight, ImageCache cache) {

    final BitmapFactory.Options options = new BitmapFactory.Options();
    ...
    BitmapFactory.decodeFile(filename, options);
    ...

    // If we're running on Honeycomb or newer, try to use inBitmap.
    if (Utils.hasHoneycomb()) {
        addInBitmapOptions(options, cache);
    }
    ...
    return BitmapFactory.decodeFile(filename, options);
  }

  private static void addInBitmapOptions(BitmapFactory.Options options,
        ImageCache cache) {
    // inBitmap only works with mutable bitmaps, so force the decoder to
    // return mutable bitmaps.
    options.inMutable = true;

    if (cache != null) {
        // Try to find a bitmap to use for inBitmap.
        Bitmap inBitmap = cache.getBitmapFromReusableSet(options);

        if (inBitmap != null) {
            // If a suitable bitmap has been found, set it as the value of
            // inBitmap.
            options.inBitmap = inBitmap;
        }
    }
}

// This method iterates through the reusable bitmaps, looking for one
// to use for inBitmap:
protected Bitmap getBitmapFromReusableSet(BitmapFactory.Options options) {
        Bitmap bitmap = null;

    if (reusableBitmaps != null && !reusableBitmaps.isEmpty()) {
        synchronized (reusableBitmaps) {
            final Iterator<SoftReference<Bitmap>> iterator
                    = reusableBitmaps.iterator();
            Bitmap item;

            while (iterator.hasNext()) {
                item = iterator.next().get();

                if (null != item && item.isMutable()) {
                    // Check to see it the item can be used for inBitmap.
                    if (canUseForInBitmap(item, options)) {
                        bitmap = item;

                        // Remove from reusable set so it can't be used again.
                        iterator.remove();
                        break;
                    }
                } else {
                    // Remove from the set if the reference has been cleared.
                    iterator.remove();
                }
            }
        }
    }
    return bitmap;
  }
  ```
- option 2) use ```internal inmemory cache```
  code snippet
  ```
  private static IDictionary<int, Bitmap> _cachedBitmap = new Dictionary<int, Bitmap>();
  private static Bitmap MemoryCache(int resId, int samplingWidth, int samplingHeight, int scale)
  {
      try
      {
          if (_cachedBitmap.ContainsKey(resId))
          {
              Console.WriteLine($"{resourceId} is in cache.");
              return _cachedBitmap[resId];
          }

          var bitmap = DecodeSampledBitmapFromResource(context.Resources, 
              resId, 
              samplingWidth,
              samplingHeight);

          var bitmapScaled = bitmap.Resize(scale);
              _cachedBitmap.Add(resId, bitmapScaled);
              return bitmapScaled;
      }
      catch (Exception ex)
      {
          MvvmCross.Mvx.IoCProvider.Resolve<IErrorHandler>().Error($"error - {resId}", ex);
      }
      return default;
  }
  ```

