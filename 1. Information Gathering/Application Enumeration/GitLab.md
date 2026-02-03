#### Version
The only way to footprint the GitLab version number in use is by browsing to the `/help` page when logged in.
If the GitLab instance allows us to register an account, we can log in and browse to this page to confirm the version.
#### User enumeration (Gitlab 13.10.3)
If we cannot register an account, we may have to try a low-risk exploit such as [this](https://www.exploit-db.com/exploits/49821).
## Enumeration
The first thing we should try is browsing to `/explore` and see if there are any public projects that may contain something interesting.

From here, we can explore each of the pages linked in the top left `groups`, `snippets`, and `help`. We can also use the search functionality and see if we can uncover any other projects.

Suppose the organization did not set up GitLab only to allow company emails to register or require an admin to approve a new account. In that case, we may be able to access additional data.

We can also use the registration form to enumerate valid users (more on this in the next section). If we can make a list of valid users, we could attempt to guess weak passwords or possibly re-use credentials that we find from a password dump using a tool such as `Dehashed` as seen in the osTicket section.

Even if the `Sign-up enabled` checkbox is cleared within the settings page under `Sign-up restrictions`, we can still browse to the `/users/sign_up` page and enumerate users but will not be able to register a user.
#### Hunt for sensitive data in git repositories
As this [blog post](https://tillsongalloway.com/finding-sensitive-information-on-github/index.html) explains, there is a considerable amount of data that we may be able to uncover on GitLab, GitHub, etc.