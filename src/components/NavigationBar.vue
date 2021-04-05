<template>
  <div class="wrapper-fluid wrapper-navbar sticky-navbar" id="wrapper-navbar">
    <nav class="navbar navbar-expand-lg navbar-light">
      <div class="container">
        <!-- Your site title as branding in the menu -->
        <a
          href="#"
          class="navbar-brand custom-logo-link"
          rel="home"
          itemprop="url"
          ><img
            src="book.png"
            class="img-fluid"
            alt="Search Medieval Manuscripts"
            itemprop="logo"
        /></a>
        <a
          class="navbar-brand site-title-with-logo"
          rel="home"
          href="/"
          title="MMP"
          itemprop="url"
          >MMP</a
        >
        <button
          class="navbar-toggler"
          type="button"
          data-toggle="collapse"
          data-target="#navbarNavDropdown"
          aria-controls="navbarNavDropdown"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div
          class="collapse navbar-collapse justify-content-end"
          id="navbarNavDropdown"
        >
          <!-- The Menu goes here -->
          <ul id="main-menu" class="navbar-nav">
            <li id="menu-item-11" class="menu-item nav-item">
              <a title="Home" href="/" class="nav-link">Home</a>
            </li>
            <li id="menu-item-15" class="menu-item nav-item">
              <a
                target="_blank"
                title="About"
                href="https://mmp.acdh-dev.oeaw.ac.at/info/about-the-project/"
                class="nav-link"
                >About</a
              >
            </li>
            <li id="menu-item-11" class="menu-item nav-item">
              <a
                target="_blank"
                title="Home"
                href="https://github.com/anturom/mmp-search"
                class="nav-link"
                >Source code</a
              >
            </li>
          </ul>
        </div>
      </div>
      <!-- .collapse navbar-collapse -->
    </nav>
  </div>
</template>