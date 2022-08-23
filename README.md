# Pull Request Stats

[![CI](https://github.com/flowwer-dev/pull-request-stats/workflows/Tests/badge.svg)](https://github.com/flowwer-dev/pull-request-stats/actions?query=workflow%3ATests)
[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-Pull%20Request%20Stats-blue.svg?colorA=24292e&colorB=0366d6&style=flat&longCache=true&logo=data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA4AAAAOCAYAAAAfSC3RAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAM6wAADOsB5dZE0gAAABl0RVh0U29mdHdhcmUAd3d3Lmlua3NjYXBlLm9yZ5vuPBoAAAERSURBVCiRhZG/SsMxFEZPfsVJ61jbxaF0cRQRcRJ9hlYn30IHN/+9iquDCOIsblIrOjqKgy5aKoJQj4O3EEtbPwhJbr6Te28CmdSKeqzeqr0YbfVIrTBKakvtOl5dtTkK+v4HfA9PEyBFCY9AGVgCBLaBp1jPAyfAJ/AAdIEG0dNAiyP7+K1qIfMdonZic6+WJoBJvQlvuwDqcXadUuqPA1NKAlexbRTAIMvMOCjTbMwl1LtI/6KWJ5Q6rT6Ht1MA58AX8Apcqqt5r2qhrgAXQC3CZ6i1+KMd9TRu3MvA3aH/fFPnBodb6oe6HM8+lYHrGdRXW8M9bMZtPXUji69lmf5Cmamq7quNLFZXD9Rq7v0Bpc1o/tp0fisAAAAASUVORK5CYII=)](https://github.com/marketplace/actions/pull-request-stats)

Github action to print relevant stats about Pull Request reviewers.

The objective of this action is to:

* Reduce the time taken to review the pull requests.
* Encourage quality on reviews.
* Help deciding which people to assign as reviewers.

Running this action will add a section at the bottom of your pull requests description:

![](/assets/pull-request.png)

Or integrate this action with **Slack**:

![](/assets/slack.png)

## Privacy
* **No repository data is collected**, stored or distributed by this GitHub action. This action is **state-less**.
* Charts data is send over the URL, and never stored or transmitted anywhere else.
* [Minimal data](/src/services/telemetry/sendStart.js) is send to Mixpanel in order to improve this action. However, you can opt-out using `telemetry` option.

## Usage

Just add this action to one of your [workflow files](https://docs.github.com/en/actions/configuring-and-managing-workflows/configuring-a-workflow):

```yml
      - name: Run pull request stats
        uses: flowwer-dev/pull-request-stats@master
```

### Action inputs

The possible inputs for this action are:

| Parameter | Description | Default |
| --------- | ----------- | ------- |
| `token` | A [Personal Access Token](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token) with `repo` permissions. Required to calculate stats for an organization or multiple repos. | `GITHUB_TOKEN` |
| `repositories` | A comma separated list of github repositories to calculate the stats. When specifying other repo(s) **it is mandatory to pass a Personal Access Token** in the `token` parameter. | Current repository |
| `organization` | If you prefer, you may specify the name of your organization to calculate the stats across all of its repos. When specifying an organization **it is mandatory to pass a Personal Access Token** in the `token` parameter. | `null`|
| `period` | The length of the period used to calculate the stats, expressed in days. | `30` |
| `limit` | The maximum number of rows to display in the table. A value of `0` means unlimited. |`0`|
| `charts` | Whether to add a chart to the start or not. Possible values: `true` or `false`. | `false` |
| `disable-links` | If `true`, removes the links to the detailed charts. Possible values: `true` or `false`. | `false` |
| `sort-by` | The column used to sort the data. Possible values: `REVIEWS`, `TIME`, `COMMENTS`. | `REVIEWS` |
| `publish-as` | Where to publish the results. Possible values: as a `COMMENT`, on the pull request `DESCRIPTION`. | `COMMENT` |
| `telemetry` | Indicates if the action is allowed to send monitoring data to the developer. This data is [minimal](/src/services/telemetry/sendStart.js) and helps me improve this action. **This option is a premium feature reserved for [sponsors](#premium-features-).** |`true`|
| `slack-webhook` | A Slack webhook URL to post resulting stats. **This option is a premium feature reserved for [sponsors](#premium-features-).** |`null`|
| `slack-channel` | The Slack channel where stats will be posted. Include the `#` character (eg. `#mychannel`). Required when a `slack-webhook` is configured. |`null`|


## Examples

**Minimal config**

Add this to the file `.github/workflows/stats.yml` in your repo:

```yml
name: Pull Request Stats

on:
  pull_request:
    types: [opened]

jobs:
  stats:
    runs-on: ubuntu-latest
    steps:
      - name: Run pull request stats
        uses: flowwer-dev/pull-request-stats@master
```

This config will:

* Calculate the reviewer stats for the current repo in the lasts 30 days
* Add links to the historial data
* Sort results by the "total reviews" column by default

and print a table like this:

|                                                                                                                                                                    | User          | Total reviews | Median time to review                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Total comments |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | -------------- |
| <a href="https://github.com/jartmez"><img src="https://avatars.githubusercontent.com/u/8755542?u=5f845c5d64ccdef5da89024edd22fcbb306bad82&v=4" width="20"></a> | jartmez | **37**        | **22m**                                                                                                                                       | 13             |
| <a href="https://github.com/manuelmhtr"><img src="https://avatars.githubusercontent.com/u/1031639?u=30204017b73f7a1f08005cb8ead3f70b0410486c&v=4" width="20"></a>    | manuelmhtr    | 35            | 48m                                                   | **96**         |
| <a href="https://github.com/ernestognw"><img src="https://avatars.githubusercontent.com/u/33379285?u=c50ed2928058edc5d412af3d9b9045f6e3309970&v=4" width="20"></a>   | ernestognw    | 25            | 1h 27m                                                                                                                                                                                                                                         | 63             |
| <a href="https://github.com/javierbyte"><img src="https://avatars.githubusercontent.com/u/2009676?u=9aa491152ac3aba42ef8c485cb5331f48bc2fce6&v=4" width="20"></a>      | javierbyte       | 12            | 30m                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | 0              |
| <a href="https://github.com/Phaze1D"><img src="https://avatars.githubusercontent.com/u/8495952?v=4" width="20"></a>                                                   | Phaze1D       | 4             | 34m                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | 1              |


**Visual config**

Add this to the file `.github/workflows/stats.yml`:

```yml
name: Pull Request Stats

on:
  pull_request:
    types: [opened]

jobs:
  stats:
    runs-on: ubuntu-latest
    steps:
      - name: Run pull request stats
        uses: flowwer-dev/pull-request-stats@master
        with:
          token: ${{ secrets.ADD_A_PERSONAL_ACCESS_TOKEN }}
          organization: 'piedpiper'
          period: 7
          charts: true
          disable-links: true
          sort-by: 'COMMENTS'
```

This config will:

* Calculate the reviewer stats for all the repos in the "piedpiper" organization in the lasts 7 days
* Display charts for the metrics
* Remove the links to detailed charts
* Sort results by the "comments" column

and print a table like this:

|                                                                                                                                                                     | User                 | Total comments      | Total reviews      | Median time to review     |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------- | ------------------------- | ------------------ | ------------------- |
| <a href="https://github.com/manuelmhtr"><img src="https://avatars2.githubusercontent.com/u/1031639?u=30204017b73f7a1f08005cb8ead3f70b0410486c&v=4" width="32"></a>    | manuelmhtr<br/>🥇    | **12**<br/>▀▀▀▀▀▀▀▀ | **8**<br/>▀▀▀▀     | 53m<br/>                  |
| <a href="https://github.com/jartmez"><img src="https://avatars0.githubusercontent.com/u/8755542?v=4" width="32"></a>                                                  | jartmez<br/>🥈       | 3<br/>▀▀            | 4<br/>▀▀           | 58m<br/>                  |
| <a href="https://github.com/JohanAlvarado"><img src="https://avatars1.githubusercontent.com/u/4240201?u=5f845c5d64ccdef5da89024edd22fcbb306bad82&v=4" width="32"></a> | JohanAlvarado<br/>🥉 | 1<br/>▀             | 2<br/>▀            | 1d 16h 18m<br/>▀▀▀▀▀▀     |
| <a href="https://github.com/Estebes10"><img src="https://avatars1.githubusercontent.com/u/22161828?v=4" width="32"></a>                                               | Estebes10<br/>       | 1<br/>▀             | 1<br/>             | **19m**<br/>              |
| <a href="https://github.com/ernestognw"><img src="https://avatars1.githubusercontent.com/u/33379285?v=4" width="32"></a>                                              | ernestognw<br/>      | 0<br/>              | 2<br/>▀            | 2h 15m<br/>               |
| <a href="https://github.com/Phaze1D"><img src="https://avatars1.githubusercontent.com/u/8495952?u=19bbf940d00c110d3ca5db5abd00684fa1fad8d3&v=4" width="32"></a>       | Phaze1D<br/>         | 0<br/>              | 3<br/>▀            | 1h 28m<br/>               |
| <a href="https://github.com/javierbyte"><img src="https://avatars0.githubusercontent.com/u/2009676?u=701513ff4a6b0b7a33f4ad155de43f2fff916a6d&v=4" width="32"></a>    | javierbyte<br/>      | 0<br/>              | 1<br/>             | 21h 24m<br/>▀▀▀           |

## Stats

The stats are calculated as following:

* **Time to review:** It is the time taken by a reviewer from the _Pull Request publication_ or the last _Commit push_ (whatever happens last) to the first time the pull request is reviewed.
* **Time to review:** It is the **median** of the _times to review_ of all Pull Requests reviewed by a person in the period.
* **Total reviews:** It is the count of all Pull Requests reviewed by a person in the period.
* **Total comments:** It is the count of all the comments while reviewing other user's Pull Requests in the period (comments in own PRs don't count).

## Slack integration

To configure the Slack, integration:

1. [Create a webhook](https://slack.com/help/articles/115005265063-Incoming-webhooks-for-Slack) in your workspace (you must be a Slack admin). It should look like this: `https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXXXXXX`.
2. Set the `slack-webhook` (from previous step) and `slack-channel` (don't forget to include the `#` character) parameters in this action.
3. Ready to go!

Since it may be quite annoying to receive a Slack notification everytime someone creates a pull request, it is recommended to configure this action to be executed every while using the `schedule` trigger. For example, every monday at 9am UTC:

```yml
name: Pull Request Stats

on:
  schedule:
    - cron:  '0 9 * * 1'

jobs:
  stats:
    runs-on: ubuntu-latest
    steps:
      - name: Run pull request stats
        uses: flowwer-dev/pull-request-stats@master
        with:
          slack-webhook: 'https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXXXXXX'
          slack-channel: '#mystatschannel'
```

## Premium features ✨

This action offers some premium features only for sponsors:

* Disabling telemetry.
* Slack integration.

No minimum amount is required for now. In the future, a minimum monthly sponsorship will be inforced to access premium features.

Suggested sponsorship is $20 usd / month. Thanks for your support! 💙

## Used by

Used by hundreds of successful teams:

| <a href="https://www.sixt.com/"><img src="https://avatars.githubusercontent.com/u/25441140?s=200&v=4" width="64"></a><br/>Sixt | <a href="https://shop.lululemon.com"><img src="https://avatars.githubusercontent.com/u/17386352?s=200&v=4" width="64"></a><br/>Lululemon | <a href="https://www.deliveryhero.com"><img src="https://avatars.githubusercontent.com/u/7225556?s=200&v=4" width="64"></a><br/>Delivery H | <a href="https://jokr.com/"><img src="https://avatars.githubusercontent.com/u/84920342?s=200&v=4" width="64"></a><br/>JOKR | <a href="http://qatalog.com/"><img src="https://avatars.githubusercontent.com/u/56023495?s=200&v=4" width="64"></a><br/>Qatalog | <a href="https://firework.tv/"><img src="https://avatars.githubusercontent.com/u/25275837?s=200&v=4" width="64"></a><br/>LOOP | <a href="https://www.usehatchapp.com/"><img src="https://avatars.githubusercontent.com/u/38331218?s=200&v=4" width="64"></a><br/>Hatch | <a href="https://www.zenfi.mx/"><img src="https://avatars.githubusercontent.com/u/68744962?s=200&v=4" width="64"></a><br/>Zenfi |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
| <a href="https://www.trivago.com/"><img src="https://avatars.githubusercontent.com/u/1481788?s=200&v=4" width="64"></a><br/>**Trivago** | <a href="https://discovery.com"><img src="https://avatars.githubusercontent.com/u/48454976?s=200&v=4" width="64"></a><br/>**Discovery** | <a href="https://www.additionwealth.com/"><img src="https://avatars.githubusercontent.com/u/86253902?s=200&v=4" width="64"></a><br/>**Addition** | <a href="https://fauna.com/"><img src="https://avatars.githubusercontent.com/u/1477000?s=200&v=4" width="64"></a><br/>**Fauna** | <a href="http://open.cdc.gov/"><img src="https://avatars.githubusercontent.com/u/12104975?s=200&v=4" width="64"></a><br/>**CDC** | <a href="https://www.wecasa.fr/"><img src="https://avatars.githubusercontent.com/u/56955553?s=200&v=4" width="64"></a><br/>**Wecasa** | <a href="https://bolt.eu/"><img src="https://avatars.githubusercontent.com/u/37693190?s=200&v=4" width="64"></a><br/>**Bolt** | <a href="https://republic.com/"><img src="https://avatars.githubusercontent.com/u/18252987?s=200&v=4" width="64"></a><br/>**Republic** |

## Author

|<a href="https://github.com/manuelmhtr"><img src="https://avatars.githubusercontent.com/u/1031639?v=4" width="32"></a>|[@manuelmhtr](https://github.com/manuelmhtr)<br/>🇲🇽 Guadalajara, MX|
| -- | :-- |


## Help

This project is maintained by a single person, considering supporting the project by:

* ⭐ Star this repo.
* Sharing your [feedback](https://github.com/flowwer-dev/pull-request-stats/discussions/new).
* Joining the [community](https://discord.gg/SGYbZkac).
* Becoming a [sponsor](https://github.com/sponsors/manuelmhtr).

### License

MIT
