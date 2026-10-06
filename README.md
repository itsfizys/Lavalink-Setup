<div align="center">
  <h1>Lavalink Setup</h1>
  <p>A ready-to-run Lavalink 4 server with configured sources and plugins.</p>
  <p>
    <a href="https://github.com/itsfizys/Lavalink-Setup">Repository</a>
    &nbsp;|&nbsp;
    <a href="https://github.com/lavalink-devs/Lavalink">Lavalink</a>
    &nbsp;|&nbsp;
    <a href="https://github.com/lavalink-devs/youtube-source">YouTube Source</a>
  </p>
</div>

<hr>

<h2 id="overview">Overview</h2>

This repository contains a Lavalink 4 server with YouTube, LavaSearch, and
PulseLink plugins configured in
[`application.yml`](application.yml).

<table>
  <tr>
    <td><strong>Server</strong></td>
    <td>Lavalink 4.2.2</td>
  </tr>
  <tr>
    <td><strong>Java</strong></td>
    <td>17 or newer</td>
  </tr>
  <tr>
    <td><strong>Port</strong></td>
    <td><code>80</code></td>
  </tr>
  <tr>
    <td><strong>Configuration</strong></td>
    <td><code>application.yml</code></td>
  </tr>
</table>

<hr>

<h2 id="quick-start">Quick start</h2>

<ol>
  <li>Install Java 17 or newer.</li>
  <li>Keep <code>Lavalink.jar</code> and <code>application.yml</code> together.</li>
  <li>Set a private password under <code>lavalink.server.password</code>.</li>
  <li>Start Lavalink.</li>
</ol>

```bash
java -jar Lavalink.jar
```

Lavalink reads `application.yml` automatically from the current directory.
Before connecting a client, configure it with the same Lavalink password.

<h3>First-time configuration</h3>

The main values to configure are:

```yaml
server:
  port: 80

lavalink:
  server:
    password: change-this

plugins:
  youtube:
    oauth:
      refreshToken:
      skipInitialization: false
```

Change the password before exposing the server publicly. Do not commit
personal OAuth tokens or production credentials to the repository.

<hr>

<h2 id="youtube-oauth">YouTube OAuth</h2>

The YouTube plugin can use OAuth to make requests appear more like normal
account traffic. This is not guaranteed to prevent YouTube rate limits or
account action. Use a separate burner account, not a primary Google account,
and avoid high-traffic usage.

The current configuration is ready for the first OAuth flow:

```yaml
plugins:
  youtube:
    enabled: true
    oauth:
      enabled: true
      refreshToken:
      skipInitialization: false
```

<h3>First-time OAuth flow</h3>

<ol>
  <li>Leave <code>refreshToken</code> empty.</li>
  <li>Set <code>skipInitialization: false</code>.</li>
  <li>Start Lavalink with <code>java -jar Lavalink.jar</code>.</li>
  <li>Watch the console for the YouTube OAuth instructions.</li>
  <li>Open the official URL shown by the console and complete verification using the burner account.</li>
  <li>Copy the refresh token printed in the Lavalink console.</li>
  <li>Stop Lavalink.</li>
  <li>Paste the token into <code>plugins.youtube.oauth.refreshToken</code>.</li>
  <li>Set <code>skipInitialization: true</code>.</li>
  <li>Start Lavalink again.</li>
</ol>

```yaml
oauth:
  enabled: true
  refreshToken: paste-your-refresh-token-here
  skipInitialization: true
```

<blockquote>
  The console provides a <strong>refresh token</strong>, not a short-lived
  access token. This configuration uses <code>refreshToken</code>.
</blockquote>

If no token is available yet, keep `refreshToken` empty and use
`skipInitialization: false` while completing the OAuth flow. The related
YouTube OAuth logger is already set to `INFO`.

<hr>

<h2 id="sources">Configured sources</h2>

These tables show which sources are enabled in `application.yml` and what is
needed to enable the others.

<h3>Built-in Lavalink sources</h3>

| Source | Status | Notes |
| --- | --- | --- |
| Bandcamp | Enabled | Direct source |
| HTTP | Enabled | Direct HTTP audio sources |
| Local files | Disabled | `local: false` |
| NicoNico | Enabled | Direct source |
| SoundCloud | Enabled | Direct source |
| Twitch | Enabled | Direct source |
| Vimeo | Enabled | Direct source |
| YouTube built-in source | Disabled | The YouTube plugin is used instead |

<h3>PulseLink sources</h3>

| Source | Status | Notes |
| --- | --- | --- |
| Spotify | Enabled | — |
| Apple Music | Enabled | — |
| Tidal | Enabled | — |
| JioSaavn | Enabled | — |
| Audiomack | Enabled | — |
| Gaana | Enabled | — |
| SoundCloud | Enabled | — |
| Pandora | Enabled | — |
| Amazon Music | Disabled | Upstream search returned errors during testing |
| PulseLink YouTube search adapter | Disabled | The standalone YouTube plugin handles `ytsearch` |
| Deezer | Disabled | Requires credentials |
| Yandex Music | Disabled | Requires credentials |
| VK Music | Disabled | Requires credentials |
| Qobuz | Disabled | Requires credentials |
| yt-dlp | Disabled | The executable is not installed |
| Flowery TTS | Disabled | Requires a default voice setting |

Spotify track mirroring uses the standalone YouTube Source plugin through the
configured `ytsearch` providers. PulseLink's separate YouTube search and lyrics
adapter is disabled.

<h3>LavaSearch indexes</h3>

| Index |
| --- |
| Spotify |
| YouTube |

LavaSearch provides search functionality; it is not itself a replacement for
every source's playback implementation.

<hr>

<h2 id="plugins">Plugins and credits</h2>

These are the plugin declarations currently pinned in `application.yml`.

| Plugin | Current version | Repository | Credit |
| --- | --- | --- | --- |
| YouTube Source | `f45bbb7aebfcbc1c553769e04af6cd43afa8b7c3` snapshot | [lavalink-devs/youtube-source](https://github.com/lavalink-devs/youtube-source) | Lavalink Devs and repository contributors |
| PulseLink | `v1.7.4` | [ItzRandom23/PulseLink](https://github.com/ItzRandom23/PulseLink) | ItzRandom23 and contributors |
| LavaSearch | `1.0.0` | [topi314/LavaSearch](https://github.com/topi314/LavaSearch) | topi314 and contributors |

<p>
  <strong>Plugin repositories:</strong>
  <a href="https://maven.lavalink.dev/releases">Lavalink releases</a>
  &nbsp;|&nbsp;
  <a href="https://maven.lavalink.dev/snapshots">Lavalink snapshots</a>
  &nbsp;|&nbsp;
  <a href="https://jitpack.io">JitPack</a>
</p>

Refer to each upstream repository for current licenses, release notes,
compatibility requirements, and contribution credits.

<hr>

<h2 id="remote-cipher">Remote cipher</h2>

The YouTube plugin uses this remote cipher service:

```yaml
remoteCipher:
  url: https://cipher.kikkia.dev/
  userAgent: lavalink-server
```

This endpoint is external to this repository. Check its availability and
terms before relying on it in production.

<hr>

<h2 id="updating-plugins">Updating plugin versions</h2>

Plugin versions are intentionally pinned. Lavalink's plugin configuration
expects a concrete Maven version or snapshot identifier; it does not provide a
reliable `latest` setting for automatically selecting the newest compatible
plugin.

Dynamic values such as `latest`, `latest.release`, or `+` are not recommended
because:

<ul>
  <li>A new release can introduce breaking changes.</li>
  <li>A plugin may no longer support the installed Lavalink version.</li>
  <li>Snapshot builds can change without warning.</li>
  <li>A restart could download different code from the same configuration.</li>
  <li>Failures become harder to reproduce.</li>
</ul>

<h3>Safe update process</h3>

<ol>
  <li>Open the plugin's upstream repository and Releases page.</li>
  <li>Check the release notes and required Lavalink version.</li>
  <li>Replace only the version in <code>application.yml</code>.</li>
  <li>Set <code>snapshot: true</code> only when using the snapshot repository.</li>
  <li>Start Lavalink and check the startup logs for plugin loading errors.</li>
  <li>Test searching and playback before using the update in production.</li>
</ol>

The YouTube plugin uses a pinned snapshot commit because YouTube source
compatibility can change quickly. Leave it pinned unless a newer compatible
version has been tested.

<hr>

<h2 id="files">Repository files</h2>

<table>
  <tr>
    <td><code>Lavalink.jar</code></td>
    <td>Lavalink server binary</td>
  </tr>
  <tr>
    <td><code>application.yml</code></td>
    <td>Server, source, plugin, OAuth, logging, and cipher settings</td>
  </tr>
  <tr>
    <td><code>README.md</code></td>
    <td>Setup and configuration guide</td>
  </tr>
</table>

<div align="center">
  <sub>Configuration maintained for the Lavalink community.</sub>
</div>
