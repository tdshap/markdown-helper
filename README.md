# Targeting

Targeting values get passed in each ad call, and help the ad server choose the
most appropriate ad for the user/page/context.

Targeting can be added for the whole `page` or for individual `slots`.

Typical targeting can look like:

```
const targets = {
  env_device_type: device,
  rdt_device_template: `${device}_${template_type}`,
  ctx_template: template_type,
  ctx_page_channel: channel,
  ctx_page_sub_channel: sub_channel,
  ctx_page_slug: slug,
  env_server: server,
  cnt_tags: tags
};
```

The targets will get sent along as query parameters in the ad request.

## Events

Page level targeting: `ads.page.targeting.added`

example page level targeting payload
```
{
  targets: {
    key: val
  }
}
```

Slot level targeting: `ads.slot.targeting.added`

example slot level targeting payload
```
{
  id: 'SLOT_ID',
  targets: {
    key: val
  }
}
```



### PAGE TARGETING VALUES

Page Key | Description | Possible values
--- | --- | ---
env_device_type | Top level category of device  desktop, tablet, mobile | ---
rdt_device_template | --- | ---
ctx_template | The template used to display the page article, index, homepage | ---
ctx_page_channel | Top level content category | ---
ctx_page_sub_channel | Secondary content category level  mens | ---
ctx_page_slug | Slug of the page | ---
env_server | Deployment server environment ci, staging, production | ---
cnt_tags | Minimal set of targeting values regarding the page content (edit-supplied via the CMS) | ---
ctx_cns_version | --- | ---
ctx_ses_soc | A social network name a user came from. This value is stored across the session. | ---
ctx_ref_soc | A social network name a user came from. | ---
ctx_ref_url | URL the user came from or blank if its a new browser session | ---
usr_bkt_eva | A bucket id for ever (until the user clears their cookies)  1-100 | ---
usr_bkt_ses | A bucket id per session 1-100 N/A | ---
usr_bkt_pv | A bucket id per page view 1-100 | ---

USER DATA | --- | ---
usr_pvc_bs | Page views in current browser session | 123
usr_pvc_24hr | Page views in the last 24 hrs | 123
usr_pvc_30d | Page views in the last 30 days | 123
usr_svc_30d | Sessions in the last 30 days | 123
mbid | Media buy - Usually added to links in newsletters and other external traffic drivers to identify the traffic or campaign source. | ---
sqt | --- | ---
sqt24hrs | --- | ---
mbid | --- | ---


### SLOT TARGETING VALUES

Slot Key | Possible values | Description
--- | --- | ---
ctx_slot_instance | The instance name of the slot | inline_video_0
ctx_slot_type | The slot-type as described in the config | ---
ctx_slot_name | slot name + slot instance number | ---
ctx_slot_rn | Number of times of a slot's in-view refreshed during a session or the request number | ---
ctx_slot_manual_rn | Number of times of a slot's programmatic refreshed by api call | ---
ctx_advertisers | A list of advertiser IDs that were on the page at the time of this ad request | ---
ctx_line_items |  A list of line item IDs that were on the page at the time of this ad request | ---
ctx_creatives | A list of creative IDs that were on the page at the time of this ad request | ---







See full description of values on the [wiki](https://cnissues.atlassian.net/wiki/spaces/ATP/pages/37060702/AdOps+Targeting+Guide)

