<template>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link
    href="https://fonts.googleapis.com/css2?family=Merriweather:ital,wght@1,700&family=Shizuru&display=swap"
    rel="stylesheet"
  />
  <div id="header">
    <h1>SUM IT UP</h1>
  </div>
  <!-- <div id="left"></div>
  <div id="right"></div> -->
  <div id="wrap">
    <section>
      <span
        >Welocome my friend. I've got here for you one of these mathematic logic
        games. Point of this game is to add two numbers and get searched result
        (picked by current level). You can make operation by drag and droping
        numbers into marked area at right side of the canvas below. If your 2
        picked numbers are correct by current operation, area will turn green
        and both numbers diseaper. If your 2 picked numbers are incorrect by
        current operation, area will turn red and both numbers will return into
        left area. When you will clear whole left area, you've got new task to
        do, NEXT LEVEL! Enjoy.</span
      >
    </section>
  </div>
</template>

<script>
export default {};
</script>

<style scoped>
h1 {
  text-align: center;
  font-size: 400%;
  word-spacing: -2rem;
  color: white;
  font-family: "Shizuru", cursive;
  text-shadow: 0 2px 8px rgba(255, 255, 255, 0.808);
}
section {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  text-align: center;
  width: 70%;
  padding: 1rem;
  padding-bottom: 2rem;
}
#header {
  width: 100vw;
  background: black;
  margin-top: -2.5%;
  border-bottom: 5px solid white;
}
#wrap {
  display: flex;
  align-items: center;
  justify-content: center;
  background: black;
  color: white;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.247);
  width: 80%;
  border-radius: 0 0 2rem 2rem;
  border: 5px solid white;
  border-top: 0;
}
span {
  line-break: auto;
  font-family: "Merriweather", serif;
  font-size: 1.4em;
  word-spacing: 0.5rem;
}
@media print {
  #wrap {
    width: 90%;
  }
}
</style>
