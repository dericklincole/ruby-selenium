# How to Solve CAPTCHA With Selenium in Ruby

![](https://assets.capsolver.com/prod/posts/captcha-selenium-ruby/D9v2aOgqJCOP-d2b5ca33bd970f64a6301fa75ae2eb22.png)


CAPTCHAs, or Completely Automated Public Turing tests to tell Computers and Humans Apart, are designed to protect websites from automated bots. While they serve a crucial role in securing online platforms, they can be a significant hurdle when automating tasks with tools like Selenium. If you’re working with Selenium in Ruby and need to solve CAPTCHAs, this guide will provide a step-by-step approach to handling them effectively.

## What are Selenium and Ruby?

Before we dive into solving CAPTCHAs, it’s essential to understand the tools you’ll be working with: [Selenium](https://www.selenium.dev/) and Ruby.

- **Selenium** is a powerful open-source tool used for automating web browsers. It allows developers to write scripts in various programming languages to simulate user interactions with web pages, making it a popular choice for testing and web scraping.
- **Ruby** is a dynamic, object-oriented programming language known for its simplicity and productivity. It’s often used in web development, and when combined with Selenium, it offers a robust framework for automating browser tasks.


## Understanding CAPTCHAs and Their Types
Before diving into the solution, it’s important to understand the different types of CAPTCHAs you might encounter:

- **ImageToText CAPTCHA**s: These require the user to enter characters displayed in a distorted image. You can find some common cases [here](https://docs.capsolver.com/en/guide/recognition/ImageToTextTask/)
- **Image-based CAPTCHA**s: Users need to select images that match a given criterion (e.g., select all images with traffic lights). Mostly from reCAPTCHA
- **reCAPTCHA**: Google's advanced CAPTCHA system that often requires recognizing objects in images or simply clicking a checkbox to prove you’re not a bot.
![](https://assets.capsolver.com/prod/posts/captcha-selenium-ruby/BAn0JOEvTopm-d2b5ca33bd970f64a6301fa75ae2eb22.png)

- **hCAPTCHA**: Similar to reCAPTCHA, but often used by websites aiming for more privacy-focused solutions.

> Claim Your   <u>**Bonus Code**</u> for top captcha solutions; [CapSolver](https://www.capsolver.com/?utm_source=official&utm_medium=blog&utm_campaign=seleniumruby): **WEBS**. After redeeming it, you will get an extra 5% bonus after each recharge, Unlimited
> 
> ![](https://assets.capsolver.com/prod/images/post/2024-03-29/fbc29472-886c-45b2-9eb2-2b307f6d9700.png)
> 
## Can Selenium Ruby Solve CAPTCHAs?

One of the most common questions among developers is whether Selenium with Ruby can solve CAPTCHAs. The short answer is: not directly. Selenium alone does not have built-in capabilities to solve CAPTCHAs because they are specifically designed to differentiate between human users and bots.
However, there are several approaches to handling CAPTCHAs in Selenium Ruby:
1. **Manual Intervention**: In some cases, developers manually solve the CAPTCHA during the automation process. However, this defeats the purpose of full automation.
2. **Third-Party CAPTCHA Solvers:** The most effective method is integrating third-party services like [CapSolver](https://capsolver.com/?utm_source=official&utm_medium=blog&utm_campaign=seleniumruby) that specialize in solving CAPTCHAs using advanced algorithms and human intelligence.
3. **Solving Simple CAPTCHAs**: For very basic text CAPTCHAs, developers might write custom scripts to recognize patterns, though this approach is limited and often unreliable.

While Selenium Ruby cannot solve CAPTCHAs on its own, with the right tools and services, it’s entirely possible to automate the process of bypassing CAPTCHAs, which we’ll explore in this guide.

## Setting Up Selenium in Ruby


### Preparation

- **[Google Chrome](https://www.google.com/chrome/)**: Install the latest version of Chrome browser, as we will be using code to interact with Chrome.
- **[Ruby](https://www.ruby-lang.org/)**: Ensure that Ruby is installed on your computer.
- **[Selenium-webdriver](https://rubygems.org/gems/selenium-webdriver)**: The Ruby library for the automation tool Selenium.
- **[CapSolver](https://docs.capsolver.com/)**: The official CapSolver documentation will help you solve CAPTCHAs.

Once Ruby is installed on your computer, you can install the Selenium WebDriver library by running the command `gem install selenium-webdriver`. Check your Chrome version, and based on that, download the corresponding `chromedriver.exe` driver. You can find the download links at the following locations:
- **[Download link 1](https://googlechromelabs.github.io/chrome-for-testing/)**: Provides the latest Stable, Beta, Dev, and Canary versions of the driver.
- **[Download link 2](https://googlechromelabs.github.io/chrome-for-testing/known-good-versions-with-downloads.json)**: Provides all drivers from version 113 onwards.
- **[Download link 3](https://chromedriver.storage.googleapis.com/index.html)**: Provides all drivers for versions before 113.

### Analyzing the Target Website

We will use the website `https://recaptcha-demo.appspot.com/recaptcha-v2-checkbox.php` as an example to solve reCAPTCHA using Ruby Selenium.

Before starting, we need to understand the basics of HTML form submission. By observing this page and opening the developer tools, we can manually solve the reCAPTCHA and then click the submit button. This action sends a POST request, submitting three fields: `ex-a`, `ex-b`, and `g-recaptcha-response`, as shown below:

![](https://assets.capsolver.com/prod/posts/captcha-selenium-ruby/iWhQaALv31g1-d2b5ca33bd970f64a6301fa75ae2eb22.png)


These three fields correspond to two input elements and one textarea element under the form in the initial HTML source code, as shown below:

![](https://assets.capsolver.com/prod/posts/captcha-selenium-ruby/3AOAfGWFZHSg-d2b5ca33bd970f64a6301fa75ae2eb22.png)


### Automating the Process with Ruby Selenium

How can we automate the entire process using Ruby Selenium? The steps are as follows:
1. Ruby drives Selenium to visit the target website.
2. Ruby calls the CapSolver API to solve the reCAPTCHA and obtain a token.
3. Change the CSS style of the textarea element from `display: none` to `display: block` to make it interactive with Selenium.
4. Simulate entering the token returned by CapSolver into the textarea element.
5. Simulate clicking the submit button to submit the form and complete the verification.

### Visiting the Target Website with Ruby Selenium

Ensure that you replace the `driver_path` in the code below with the actual path to `chromedriver` on your computer.

```ruby
require 'selenium-webdriver'

# Initialize Chrome browser options and access the target website
driver_path = "path/to/chromedriver.exe"
options = Selenium::WebDriver::Chrome::Options.new
service = Selenium::WebDriver::Service.chrome(path: driver_path)
driver = Selenium::WebDriver.for :chrome, options: options, service: service
url = "https://recaptcha-demo.appspot.com/recaptcha-v2-checkbox.php"
driver.navigate.to url
```

### Getting the Token

To use the CapSolver API, we need to provide the `websiteKey`, which can be found by searching for the keyword `data-sitekey` in the page source:

![](https://assets.capsolver.com/prod/posts/captcha-selenium-ruby/WNvXZd2JNMgT-d2b5ca33bd970f64a6301fa75ae2eb22.png)


Now, let's write the Ruby code to use the CapSolver API to automatically solve the reCAPTCHA:

```ruby
require 'net/http'
require 'json'
require 'time'

def cap_solver(api_key, public_key, page_url)
  payload = {
    "clientKey" => api_key,
    "task" => {
      "type" => 'ReCaptchaV2TaskProxyLess',
      "websiteKey" => public_key,
      "websiteURL" => page_url,
    }
  }

  # Send a task creation request
  uri = URI("https://api.capsolver.com/createTask")
  res = Net::HTTP.post(uri, payload.to_json, { "Content-Type" => "application/json" })
  resp = JSON.parse(res.body)
  task_id = resp["taskId"]

  unless task_id
    puts "Failed to create task: #{res.body}"
    return
  end

  puts "Got taskId: #{task_id}"

  # Loop waiting to obtain task results
  loop do
    sleep(1)
    payload = { "clientKey" => api_key, "taskId" => task_id }
    uri = URI("https://api.capsolver.com/getTaskResult")
    res = Net::HTTP.post(uri, payload.to_json, { "Content-Type" => "application/json" })
    resp = JSON.parse(res.body)
    status = resp["status"]
    if status == "ready"
      token = resp.dig("solution", "gRecaptchaResponse")
      puts "Solve succeed, token: #{token}"
      return token
    elsif status == "processing"
      puts "Solve in progress..."
    elsif status == "failed"
      puts "Solve failed! response: #{res.body}"
      return
    end
  end
end
```

### Using the Token in Selenium

Next, we need to input the token into the webpage, automatically click submit, and complete the entire process. Let's combine all the code; the complete code is as follows (be sure to replace `cap_solver_api_key` with your own key, which can be found in the CapSolver dashboard):

```ruby
require 'selenium-webdriver'
require 'net/http'
require 'json'
require 'time'

def cap_solver(api_key, website_key, page_url)
  payload = {
    "clientKey" => api_key,
    "task" => {
      "type" => 'ReCaptchaV2TaskProxyLess',
      "websiteKey" => website_key,
      "websiteURL" => page_url,
    }
  }

  # Send a task creation request
  uri = URI("https://api.capsolver.com/createTask")
  res = Net::HTTP.post(uri, payload.to_json, { "Content-Type" => "application/json" })
  resp = JSON.parse(res.body)
  task_id = resp["taskId"]

  unless task_id
    puts "Failed to create task: #{res.body}"
    return
  end

  puts "Got taskId: #{task_id}"

  # Loop waiting to obtain task results
  loop do
    sleep(1)
    payload = { "clientKey" => api_key, "taskId" => task_id }
    uri = URI("https://api.capsolver.com/getTaskResult")
    res = Net::HTTP.post(uri, payload.to_json, { "Content-Type" => "application/json" })
    resp = JSON.parse(res.body)
    status = resp["status"]
    if status == "ready"
      token = resp.dig("solution", "gRecaptchaResponse")
      puts "Solve succeed, token: #{token}"
      return token
    elsif status == "processing"
      puts "Solve in progress..."
    elsif status == "failed"
      puts "Solve failed! response: #{res.body}"
      return
    end
  end
end

# Initialize Chrome browser options and access the target website
driver_path = "path/to/chromedriver.exe"
options = Selenium::WebDriver::Chrome::Options.new
service = Selenium::WebDriver::Service.chrome(path: driver_path)
driver = Selenium::WebDriver.for :chrome, options: options, service: service
url = "https://recaptcha-demo.appspot.com/recaptcha-v2-checkbox.php"
driver.navigate.to url

# Call CapSolver API to solve ReCaptcha
cap_solver_api_key = 'YOUR_API_KEY'
website_key = '6LfW6wATAAAAAHLqO2pb8bDBahxlMxNdo9g947u9'
token = cap_solver(cap_solver_api_key, website_key, url)
if token.nil? || token.empty?
  puts "Failed to solve captcha, Press any key to exit."
  STDIN.gets
  driver.quit
  return
end

# Change the display style property of textarea to block to make it visible
driver.execute_script("document.getElementById('g-recaptcha-response').style.display = 'block';")
# Simulate inputting token into textarea
textarea = driver.find_element(id: 'g-recaptcha-response')
textarea.send_keys(token)
# Simulate clicking and submitting a form
submit_btn = driver.find_element(css: "button[type='submit']")
submit_btn.click

puts "Press any key to exit."
STDIN.gets
driver.quit
```
![](https://assets.capsolver.com/prod/posts/captcha-selenium-ruby/acNrz22gjMbB-d2b5ca33bd970f64a6301fa75ae2eb22.png)

Run the above code and you will see that the recaptcha has been successfully solved.

## More Information

[CapSolver](https://www.capsolver.com/?utm_source=official&utm_medium=blog&utm_campaign=seleniumruby) uses AI-based automatic web unlock technology to help you solve CAPTCHAs in seconds. It can solve not only reCAPTCHA but also hCaptcha, Geetest, Cloudflare Turnstile, DataDome, AWS WAF, and more. CapSolver also provides SDKs in multiple languages as well as browser extensions. You can refer to the [CapSolver documentation](https://docs.capsolver.com/?utm_source=official&utm_medium=blog&utm_campaign=seleniumruby) for more information.
