<script>
  function calculateCircumference(radius) {
    const circumference = 2 * Math.PI * radius;
    return circumference.toFixed(2);
  }
  const radius = 5; // replace 5 with your actual radius value
</script>

<p>The circumference of a circle with radius {radius}m is {calculateCircumference(radius)}m.</p>