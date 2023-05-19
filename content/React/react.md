+++
title = "react"
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## why dev mode render twice in react

In React, the development mode by default renders components twice. This is done intentionally to detect any unintended side-effects or changes that might occur during the render process.

During the first render cycle, React builds a virtual representation of the component tree and compares it with the previous render cycle. If there are any differences, it updates the real DOM accordingly.

Then, during the second render cycle, React compares the new and old virtual trees again to check if there were any changes made during the update process. This helps ensure that the updates are applied correctly, and it also helps identify any potential issues with the components or the rendering process.

It's worth noting that this behavior only occurs in development mode, and not in production mode, where the rendering process is optimized for performance.
