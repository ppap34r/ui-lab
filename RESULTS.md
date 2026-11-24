# UI lab results

Tool parts that I chose: Part 2 (Selenium WebDriver in Java) and Part 4 (Cypress)

## Part 0
- Step 0.1, Chrome version: 158.0.8127.37 (Official Build) (arm64)
- Step 0.3, ChromeDriver version: ChromeDriver 158.0.8127.37
- Step 0.4, locator of Pizza, Please!: `button[name="pizza-please"]`
- Step 0.4, locator of Excluded tools: `select` (CSS), or `//label[normalize-space()='Excluded tools']/preceding-sibling::select` (XPath) — check in the Console that it finds exactly 1
- Step 0.4, locator of Must be vegetarian: `#default-checkbox`
- Step 0.4, locator of Custom Pizza Name: `input#pizza-name`
- Step 0.5, pizza name, tool and calories of the run by hand:

| Step | Actual result | Pass or fail |
|------|---------------|--------------|
| 1    | The QuickPizza home page opened successfully, and the heading displayed "Looking to break out of your pizza routine?" as expected. |.Pass |
| 2    | The Advanced options were displayed successfully, including Max Calories per Slice, Excluded tools, Must be vegetarian, and Custom Pizza Name. | Pass |
| 3    | All restrictions and the custom pizza name were entered correctly. | Pass |
| 4    | The recommendation displayed the correct pizza name, used Scissors instead of Knife, and had 475 calories per slice. | Pass |
| 5    | After clicking the "Love it!" button without logging in, the page displayed the message "Please log in first." | Pass |
| 6    | Successfully logged in to the admin page. The page displayed the "Latest pizza recommendations" heading and a list of recommendations. | Pass |
| 7    | The pizza "Lab Pizza 20261124-130355" was successfully found in the admin list with the tool Scissors and 6 ingredients. It appeared as the newest recommendation. | Pass |

## Part 2: Selenium WebDriver in Java
- Step 2.4, the failure line for the wrong title: FirstTest.webFormOpens:20 expected: <Web Form> but was: <Web form>
- Step 2.15, QP01Test run 1, result and time: Passed (Tests run: 1, Failures: 0), 17.19 s
- Step 2.15, QP01Test run 2, result and time: Passed (Tests run: 1, Failures: 0), 9.872 s
- Step 2.20, full run 1, time and number of tests: 32.416 s, Tests run: 5, Failures: 0, Errors: 0 (finished 13:25:27)
- Step 2.20, full run 2, time and number of tests: 27.511 s, Tests run: 5, Failures: 0, Errors: 0 (finished 13:26:11)

## Part 4: Cypress
- node --version, npm --version and npx cypress version: v22.14.0, 10.9.2, Cypress package 16.2.1 / binary 16.2.1
- Step 4.3, failure message for the wrong title: Timed out retrying after 10000ms, + expected - actual, -'Web form' +'Web Form' (at web-form.cy.js:6:15)
- Step 4.6, tool and calories in the answer of POST /api/pizza: tool = Pizza cutter, calories = 450
- Step 4.7, the line of the admin list that shows your pizza: Lab Pizza 20261124-133910 (tool=Pizza cutter, ingredients_number=6)
- Step 4.8, run 1, result, duration and pizza name: all passed (2 tests, 0 failing); pizza-scenario.cy.js 00:11, web-form.cy.js 00:03; Lab Pizza 20261124-134533 (tool=Scissors, ingredients_number=8)
- Step 4.8, run 2, result, duration and pizza name: all passed (2 tests, 0 failing); pizza-scenario.cy.js 00:10, web-form.cy.js 00:02; Lab Pizza 20261124-134744 (tool=Pizza cutter, ingredients_number=6) 

## Part B
1. The three number boxes share the id `floating_filled` (the Console found 3), so the id cannot pick one box. The tests use the unique label text and take the input next to it. A unique id such as `max-calories-per-slice` would give a shorter and more stable locator.
2. Step 7 is a consistency oracle, because it checks that the recommendation and the admin list show the same tool. It would catch a server that saves a different tool from the one it shows. Step 4.6 in Cypress adds the same kind of check between the page and the API response.
3. The page does not show whether the pizza is vegetarian. I would check the API response instead: `expect(response.body.vegetarian).to.be.true`. A check on the page would need it to show the ingredients or a "Vegetarian" line.
4. Selenium needed an explicit wait for every late element, while Cypress waited by itself. Cypress was faster to write and to run (about 8 s against 10 to 17 s), and it needed no ChromeDriver, which broke when Chrome updated to 158. Cypress also gave a clearer report with a video and screenshots, but it crashed when both specs ran together in Chrome 158.
5. I would choose Cypress for Project 2. It waits for elements by itself, so the tests need less code. It also records a video and screenshots as evidence with almost no extra code.
6. In QP01Test I would edit the XPath inside the test, and in every test that copies it. In QP01PageObjectTest the test does not change. I only change `MAX_CALORIES` in HomePage, one line in one place.
7. A cookie keeps the browser on one QuickPizza server, and only that server's admin list has the new pizza. Cypress clears cookies before each test, so a separate test for steps 6 and 7 might not find the pizza. That test also could not use `pizzaName` or `@tool` from the first test.

# Note: Chrome was 156.0.8078.80 when I started, so I first downloaded ChromeDriver 156.0.8078.80.Chrome then updated itself to 158