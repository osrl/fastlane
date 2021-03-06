# We generally try to avoid big PRs
warn("Big PR") if git.lines_of_code > 500

# Show a warning for PRs that are Work In Progress
if (github.pr_body + github.pr_title).include?("WIP")
  warn("Pull Request is Work in Progress")
end

# Contributors should always provide a changelog when submitting a PR
if github.pr_body.length < 5
  warn("Please provide a changelog summary in the Pull Request description @#{github.pr_author}")
end

# We want contributors to create an issue first before submitting a PR
# Exceptions are version bumps
if !github.pr_title.downcase.include?('version bump') &&
   !github.pr_body.include?("https://github.com/fastlane/fastlane/issues/") &&
   github.pr_body.match(/#\d+/).nil?
  warn("Before submitting a Pull Request, please create an issue on GitHub to discuss the change. Please add a link to the issue in the PR body.")
end

# To avoid "PR & Runs" for which tests don't pass, we want to make spec errors more visible
# The code below will run on Circle, parses the results in JSON and posts them to the PR as comment
containing_dir = ENV["CIRCLE_TEST_REPORTS"] || "." # for local testing
file_path = File.join(containing_dir, "rspec", "fastlane-junit-results.xml")

if File.exist?(file_path)
  junit.parse(file_path)
  junit.headers = [:name, :file]
  junit.report
else
  puts "Couldn't find any test artifacts in path #{file_path}"
end
