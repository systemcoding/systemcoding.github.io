+++
title = 'Freeze Engine v0.4'
date = 2024-05-31T13:42:59+05:30
draft = false
+++

***
Hey everyone! It's been quite some time since I talked about my 2D Engine. This release isn't anything big but I think it's a pretty good one since the start of the engine. I am planning on adding more features to the engine (for v0.5). Let's get into what's new and some problems as well.

### New Features
Let's start with some new stuff, there aren't much but these will do:
- Better Batch Renderer
- Replaced Box2D Debug Renderer with Batch Renderer functions
- Removed some sandbox clutter
- Added Framebuffers
- Added OS and GPU Info to Sandbox
- Fixed a crash when calling the OnClose() Window Event
- Fixed several bugs

### **NOTE: This engine only supports Linux. I have no plans to support Windows or other OS.**

## Batch Renderer:

Not much but the most important feature is the Batch Renderer. It's already in v0.3 but the improvement is that now by default all the shapes are unit size. And then I multiply the transformation matrix to the unit sized shapes to achieve the shape I want. This idea is not mine but it was given my a user from discord (not mentioning the username here for obvious reasons). This simplifies the task and all I need to do now is just multiply the vertices of the shape and the transformation matrix. Here is some example code:

```cpp
glm::mat4 transform = glm::translate(glm::mat4(1.0f), glm::vec3(position, 0.0f))
                        * glm::scale(glm::mat4(1.0f), glm::vec3(size, 1.0f)); // Ensure correct scale along z-axis

glm::vec4 quadVertices[4] = {
    glm::vec4(-0.5f, -0.5f, 0.0f, 1.0f), 
    glm::vec4(0.5f, -0.5f, 0.0f, 1.0f),  
    glm::vec4(0.5f, 0.5f, 0.0f, 1.0f),   
    glm::vec4(-0.5f, 0.5f, 0.0f, 1.0f)   
};

// Update vertex buffer with transformed vertices
for (int i = 0; i < 4; ++i) {
    m_RendererData->QuadVertexBufferCurrent->Position = transform * quadVertices[i];
    m_RendererData->QuadVertexBufferCurrent->Color = color;
    m_RendererData->QuadVertexBufferCurrent++;
}
```

There is also another version of the DrawQuad function which takes in rotation. The above function doesn't take rotation as a parameter.
I use the [TRS](https://learnopengl.com/Getting-started/Transformations) (Translate-Rotate-Scale) method for the transformation matrix.

## Physics API:
Not many improvements but now it's possible to render the physics body using any Batch Renderer API instead of using the Render method included in the physics API (in v0.3).

```cpp
// For Example:
float entityRotation = m_Body->GetBody()->GetAngle();
Freeze::Renderer2D::DrawRotatedQuad({ m_Body->GetBody()->GetPosition().x, m_Body->GetBody()->GetPosition().y }, 
                                    { m_Body->GetBodyData()->Size.x, m_Body->GetBodyData()->Size.y }, 
                                    entityRotation,
                                    { 0.6f, 0.1f, 0.3f, 1.0f });
```

I am using the rotated quad function but you can `DrawQuad` instead of the rotated function. In the later versions I will also add 
shapes like circles and then simulating circles using Box2D. The API is not the best, there is still a lot that needs to be done
and additionally I've added a Debug Renderer (not in a seperate window or anything like that). But it basically shows the physics bodies alongside the rendering. It's a feature that I think is important as we may encounter issues like incorrect collisions, clipping and several other stuff. The Sandbox is really small, so it's not a huge problem. As it gets bigger we may encounter issues.
Below is a demo of the engine.

![image](/engine.png)

I am going to conclude this by talking about some features that were left out. Textured Quads is something that I wanted to add for a very long time. Same goes for sprite rendering. We also have a working Event system that's not yet complete but it's fine for now.
No need for complex garbage.

<br>

For audio I am using [Soloud](https://github.com/jarikomppa/soloud). Some may recommend libraries like FMOD, OpenAL etc... But Soloud seems to be working fine. The API currently is not the best (again everything is still in development). There is no text rendering but I am not really sure which library to use but I will figure it out.

### Features expected in v0.5:
- Text rendering
- Sprite rendering and simulations
- Add more geometry to the Batch Renderer
- Texture Rendering in the Batch Renderer
- Better way to visualise physics bodies (maybe using ImGui and Framebuffers)
- Extend the Physics API
- Bug fixes (if found)

Hey! Thanks for stopping by, really appreciate that you read the blog post. If you want to try the engine for yourself, checkout the [GitHub page](https://github.com/systemcoding/Freeze-Engine) or visit the [Links](/links) page. This is my first post so I won't expect it to be good. Any problems can be reported through my email. Please check out my YouTube channel for more content related to the engine and Linux stuff. Have a good day!
