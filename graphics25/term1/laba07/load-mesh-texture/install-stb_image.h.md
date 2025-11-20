# Install stb\_image.h



1. Download stb\_image.h from [https://github.com/nothings/stb/blob/master/stb\_image.h](https://github.com/nothings/stb/blob/master/stb_image.h)
2. Create a stb directory in your include directory
3. Save stb\_image.h to your stb/stb\_image.h
4. In Mesh.cpp, in the header files section, add

```cpp
#define STB_IMAGE_IMPLEMENTATION
#include <stb/stb_image.h>
```



<figure><img src="../../../../.gitbook/assets/716ac0eb-095f-41e8-ab0f-571321c74ab4 (1).png" alt=""><figcaption></figcaption></figure>
