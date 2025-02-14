---
date: 2025-02-14
---
# Steps
---
1.  Choose Blazor web App
2. Choose [[#Interactivity Type]]
	1. None: No interactivity, just static server side rendering, unless using [[Blazor Form]].
	2. Server: Just server components, not WA
	3. WebAssembly: Just WA, no server. **Works Offline** :O
	4. Auto: Both, but creates two projects
3. Interactivity location: Control interactivity globally or per page. (per page or component is better)

# Interactivity Type
---
SSR
- The code is not send to the user, it stays in the server.
- There is no interactivity with the user.
WebAssembly
- The code is send to the user in a DLL.
- There is interactivity with the user.

When using `@attribute [RenderModeInteractiveAuto]`, first renders on SSR, then on WA waiting for a user change.

On a razor component,  `@attribute [StreamRendering(true)]` allows to have a connection open on SSR. This allows for some interactivity, when a value on the server changes, it sends this change and re-renders only the component, not the whole page. 

	

# Sources
---
[Tim Corey Tutorial](https://www.youtube.com/watch?v=walv3nLTJ5g)
[Render Modes Explained](https://www.youtube.com/watch?v=C_bYPn-OTtw)
