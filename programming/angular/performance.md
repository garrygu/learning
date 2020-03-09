
# Articles
## [Top Reasons Why Your Angular App Is Slow](https://blog.bitsrc.io/top-reasons-why-your-angular-app-is-slow-c36780a0a289)
#### Your app is rendering too often  
- Change Detection  
Setting the default change detection to OnPush
- Async Pipe  
  - use operators such as filter and distinctUntilChanged to skip re-renderings altogether.  
  - only receive updates from the state slice affected.  
  - [RxJS Patterns: Efficiency and Performance](https://blog.bitsrc.io/rxjs-patterns-efficiency-and-performance-10bbf272c3fc)  
- Zone.js   
[Quantum Angular: Maximizing Performance by Removing Zone](https://blog.bitsrc.io/quantum-angular-maximizing-performance-by-removing-zone-e0eefe85b8d8)  


#### Your app is rendering too much  
- Keying: assign a key to each item of a list, and the library will re-render it only if the key has changed.
- Virtual Scrolling: Only render what the user can see.  https://material.angular.io/cdk/scrolling/overview
- Async Rendering: start rendering a limited number of items (ex, 50 out of 500), then schedule a subsequent rendering with the next 50 items using setTimeout(0) until all items are rendered.
- Lazy Rendering    https://quilljs.com
- Lazy Listeners


#### Some code… is just slow  
- Use [WebWorkers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers).     
https://twitter.com/MikeRyanDev/status/954151139968249857   
- Use [WebAssembly](https://webassembly.org/), for example using [AssemblyScript](https://github.com/AssemblyScript/assemblyscript). Read this case study from [Figma](https://www.figma.com/blog/webassembly-cut-figmas-load-time-by-3x/) for more information.
- Use a [Custom Iterable Differ](https://blog.mgechev.com/2017/11/14/angular-iterablediffer-keyvaluediffer-custom-differ-track-by-fn-performance/)
- Turn everything into for-loops, scrap filter, reduce and map. Use break and continue to reduce the number of iterations
- Maintain the shape of your objects. Learn more about how Angular is so fast [watching this video from Misko Hevery](https://www.youtube.com/watch?v=EqSRpkMRyY4)


#### Takeaways
- Opt-out of the framework’s magic: make sure you use ChangeDetection.OnPush and TrackBy for arrays
- Render less often by surgically triggering change detections on your components. Run outside Zone when needed.
- Try to render less using a variety of techniques such as virtual scrolling and lazy rendering
- Don’t listen to everything: subscribe to only the items that are visible, and subscribe to only one global event listener

https://www.youtube.com/watch?v=Tlmx1PbP8Qw&t=6s


## [Faster Angular Applications - Understanding Differs. Developing a Custom IterableDiffer](https://blog.mgechev.com/2017/11/14/angular-iterablediffer-keyvaluediffer-custom-differ-track-by-fn-performance/)
