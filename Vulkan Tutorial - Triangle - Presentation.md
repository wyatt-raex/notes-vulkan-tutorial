Links: [[Vulkan Tutorial]]
Tags: #Computer_Science #Graphics #Programming 

# Local TOC
- [[#Window Surface]]
- [[#Swap Chain]]
- [[#Image Views]]

## Window Surface
- [[#Local TOC|Back to Top]]
- [[#Window Surface Creation]]
- [[#Querying for Presentation Support]]
- [[#Creating the Presentation Queue]]

Vulkan is platform agnostic API, so it can't interface directly with the window system on it's own. To connect Vulkan to the window system in order to print results to the screen, we need to use the *WSI (Window System Integration)* extensions. [[#Window Surface|This Chapter]]  discusses the first one, `VK_KHR_Surface`, which exposes the `VkSurfaceKHR` object.
- `VkSurfaceKHR` object - <i>represents an abstract type of surface to present rendered images to</i>.
- `VK_KHR_surface` - <i>an extension existing at the instance level. We've already enabled it via</i> `glfwGetRequriedExtensions()` <i>method.</i>

The window surface needs to be created right after the instance creation, because it can actually influence the physical device selection.

### Window Surface Creation
```C++
private:
	/* ... */
	VkSurfaceKHR surface;
```
To store the created window surface we'll create a class member variable `surface`.

*Below code snippet is not implemented! See following paragraphs for explanation.*
```C++
VkWin32SurfaceCreateInfoKHR createInfo{};
createInfo.sType = VK_STRUCTURE_TYPE_WIN32_SURFACE_CREATE_INFO_KHR;
createInfo.hwnd = glfwGetWin32Window(window);
createInfo.hinstance = GetModuleHandle(nullptr);
```
As with all Vulkan Objects `VkSurfaceKHR` comes with a createInfo struct (<i>shown above</i>) to provide the necessary information in order to create the Vulkan object. However, each window surface is platform specific (different OS's and whatnot) which contrasts' Vulkan's platform agnostic behavior. If we weren't using GLFW as our Window manager we'd have to create different window surface objects based on what platform we're using. Luckily GLFW will take care of the details for us when creating a window surface via the `glfwCreateWindowSurface()` method, so we can proceed to the code below.

```C++
void initVulkan() {
	createInstance();
	setupDebugMessenger();
	createSurface(); // <-- Insert New Method Here
	pickPhysicalDevice();
	createLogicalDevice();
}

/*... */

void cleanup() {
	/* ... */
	
	vkDestroySurfaceKHR(instance, surface, nullptr);
	vkDestroyInstance(instance, nullptr);
	
	/* ... */
}

/* ... */

void createSurface() {
	if (glfwCreateWindowSurface(instance, window, nullptr, &surface) != VK_SUCCESS) {
		throw std::runtime_error("failed to create window surface!");	
	}
}
```
First we'll create a new method `createSurface()` and have it run after we've created our instance and setup our debug messenger, but crucially it runs before we do anything with physical or logical devices. Within `createSurface()` we'll then make a call to the `glfwCreateWindowSurface()` method passing it the following parameters:
- `instance` - our Vulkan instance we've created.
- `window` - the GLFW window pointer that's been created.
- `nullptr` - custom allocators (<i>which we're not including</i>).
- `&surface` - pointer to the variable that we'll store the window surface in.

Finally any object that we've created we also need to destroy, and we'll do so in the `cleanup()` method using `vkDestroySurfaceKHR()`.

### Querying for Presentation Support

### Creating the Presentation Queue

## Swap Chain
- [[#Local TOC|Back to Top]]
- [[#Checking for Swap Chain Support]]
- [[#Enabling Device Extensions]]
- [[#Querying Details of Swap Chain Support]]
- [[#Choosing the Right Settings for the Swap Chain]]
	- [[#Surface Format]]
	- [[#Presentation Mode]]
	- [[#Swap Extent]]
- [[#Creating the Swap Chain]]
- [[#Retrieving the Swap Chain Images]]

### Checking for Swap Chain Support

### Enabling Device Extensions

### Querying Details of Swap Chain Support

### Choosing the Right Settings for the Swap Chain

#### Surface Format

#### Presentation Mode

#### Swap Extent

### Creating the Swap Chain

### Retrieving the Swap Chain Images

## Image Views
- [[#Local TOC|Back to Top]]