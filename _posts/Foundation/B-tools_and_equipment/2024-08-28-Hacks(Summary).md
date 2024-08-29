<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Version Control and Deployment</title>
    <style>
        body {
            font-family: 'Times New Roman', Times, serif;
            background-color: #000;
            color: #fff;
            margin: 0;
            padding: 0;
            line-height: 1.6;
            background-image: url('');
            background-size: cover;
            background-attachment: fixed;
            background-repeat: no-repeat;
            background-position: center center;
        }

        .container {
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background-color: rgba(0, 0, 0, 0.8);
            box-shadow: 0px 0px 20px rgba(255, 255, 255, 0.5);
            border: 1px solid #444;
            margin-top: 50px;
            border-radius: 10px;
        }

        h1, h2 {
            font-family: 'Georgia', serif;
            color: #fff;
            border-bottom: 2px solid #fff;
            padding-bottom: 5px;
        }

        h1 {
            text-align: center;
            font-size: 2.5em;
            margin-bottom: 20px;
        }

        h2 {
            font-size: 1.8em;
            margin-top: 30px;
        }

        p {
            font-size: 1.2em;
            text-align: justify;
            margin-bottom: 20px;
            text-indent: 50px;
        }

        .article {
            padding: 10px 20px;
            margin-bottom: 40px;
            border-bottom: 1px solid #555;
        }

        .footer {
            text-align: center;
            padding: 20px;
            font-size: 1em;
            color: #aaa;
            border-top: 1px solid #555;
            margin-top: 40px;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>Version Control and Deployment</h1>

    <div class="article">
        <h2>Version Control</h2>
        <p>While creating my Chemistry Visualizer on my homepage, I went through several iterations of my code in order to test out different features and customize the visualization to make it more minimalistic. I used version control to track how the changes I made were reflected onto my homepage.</p>
        <p>I have my repository cloned on my desktop where I can also access my files. The files in my GitHub are updated when I push my changes from VSCode and sync my changes to the repository.</p>
        <p>I would use CSS and JavaScript to customize the default UI of the 'portfolio_2025' to include more features and make the design unique. I would also add a capture page to document screenshots of important teaching moments.</p>
    </div>

    <div class="article">
        <h2>Localhost vs Deployed Server</h2>
        <p>When cloning my repository in VSCode, I deployed a local host URL by running the command "make" on my VSCode terminal. This is a local site only to your computer, meaning no one else can see those updates.</p>
        <p>When committing and pushing to update your GitHub repository, those changes are updated to your GitHub Pages website. Since this is on a GitHub domain and is public, others can see the updates that you put onto the site.</p>
    </div>

    <div class="article">
        <h2>DNS and GitHub Pages</h2>
        <p>There is a domain for my GitHub Pages; my website is hosted on github.io. The URL for each GitHub Page is different. The website follows the syntax of username.github.io/repository_name.</p>
    </div>

    <div class="footer">
        © 2024, Nisarg's Project Hub
    </div>
</div>

</body>
</html>