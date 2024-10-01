<script>
    import { onMount } from 'svelte';
    
    let y = 0;
    let rotation = 0;
    let squareSize = 50; // Size of the square in pixels
    let rightOffset = 40; // Distance from the right edge of the screen
    let transitionDuration = 0.5; // Duration of the transition in seconds
    
    let windowHeight;
  
    function handleMouseMove(event) {
      y = event.clientY - squareSize / 2; // Centers the square vertically on the cursor
      updateRotation();
    }
    
    function updateRotation() {
      // The rotation of the square is based on the vertical position.
      // In the middle it is at 0, At the top it is -90 and at the bottom it is 90
      const middleY = windowHeight / 2;
      const maxRotation = 90;
      rotation = ((y + squareSize / 2 - middleY) / middleY) * maxRotation;
    }
  
    onMount(() => {
      window.addEventListener('mousemove', handleMouseMove);
      window.addEventListener('resize', () => {
        windowHeight = window.innerHeight;
        updateRotation();
      });
      windowHeight = window.innerHeight;
      updateRotation();
      return () => {
        window.removeEventListener('mousemove', handleMouseMove);
        window.removeEventListener('resize', updateRotation);
      };
    });
  </script>

  <div
    class="square rounded-lg bg-lime-300 border-2 border-black fixed z-20"
    style="
      right: {rightOffset}px;
      top: {y}px;
      width: {squareSize}px;
      height: {squareSize}px;
      transition: top {transitionDuration}s ease-out, transform {transitionDuration}s ease-out;
      transform: rotate({rotation}deg);
    "
  ></div>

  <div class="track fixed right-[60px] h-full top-0 w-2 border-x-2 border-black bg-purple-200 z-10"></div>
  
  <style>
    .square {
      will-change: top, transform;
    }
  </style>