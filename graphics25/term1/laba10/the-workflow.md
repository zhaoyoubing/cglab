# The Workflow

1. Render the scene to texture
2. Extract the highly bright part of the rendering from the texture, save into a second highlight texture.
3. Apply Gaussian blurring to the highlight texture.
4. Blend the original scene texture with the blurred highlight texture and render it no the screen.
