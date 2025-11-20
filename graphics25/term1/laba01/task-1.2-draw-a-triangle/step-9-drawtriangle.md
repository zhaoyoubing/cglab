# Step 9 drawTriangle()

In main.cpp, define a second function drawTriangle()

Simply call glDrawArrays()  which specifies drawing an array of triangles with a vertex number of 3 (one triangle).

## glDrawArrays

<pre class="language-cpp"><code class="lang-cpp"><strong>// glDrawArrays function signature
</strong><strong>// Please do not add this to your code
</strong><strong>void glDrawArrays(GLenum mode, GLint first, GLsizei count);
</strong></code></pre>

## Calling glDrawArrays

```cpp
void drawTriangle()
{
    glColor3f(1.0f, 0.0f, 0.0f);
    glDrawArrays(GL_TRIANGLES, 0, 3);
}
```
