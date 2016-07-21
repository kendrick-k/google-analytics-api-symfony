Google Analytics API v4 Symfony bundle
======================================

# installation

    composer require mediafigaro/google-analytics-api
    
add to /app/AppKernel.php :

    $bundles = [
        ...
        new MediaFigaro\Analytics\MediaFigaroAnalytics(),
    ];

# configuration

    media_figaro_analytics.google_analytics_json_key

Set the relative path for your json key (set it on your server, better not into your repository) from execution path, ex: /data/analytics/analytics-27cef1a4c0fd.json.

/app/config/parameters.yml

    google_analytics_json_key: "../data/analytics/analytics-27cef1a4c0fd.json"

/app/config/config.yml

    media_figaro_analytics:
        google_analytics_json_key: "%google_analytics_json_key%"
        
# Google API key

Generate the json file from https://console.developers.google.com/start/api?id=analyticsreporting.googleapis.com&credential=client_key by creating a project, check the documentation : https://developers.google.com/analytics/devguides/reporting/core/v4/quickstart/service-php.

# Google Analytics API v4

List of metrics for report building with search engine : https://developers.google.com/analytics/devguides/reporting/core/dimsmets eg. ga:sessions, ga:visits, ga:bounceRate ...

Objects : https://github.com/google/google-api-php-client-services/tree/master/AnalyticsReporting

(example : ReportData object : https://github.com/google/google-api-php-client-services/blob/master/AnalyticsReporting/ReportData.php)

Samples : https://developers.google.com/analytics/devguides/reporting/core/v4/samples

# debug

Add the debug routes for development purposes :

/app/config/routing_dev.yml

    _media_figaro_analytics:
        resource: "@MediaFigaroAnalytics/Resources/config/routing_dev.yml"

http://symfony.dev/app_dev.php/analytics-api/000000000 

000000000 = profile id that you can find in the analytics URL, p000000000 :

https://analytics.google.com/analytics/web/?hl=en&pli=1#management/Settings/a222222222w1111111111p000000000/%3Fm.page%3DPropertySettings/

Result of this debug page :

![debug](doc/debug.png)

# errors

In that 403 error case, follow the link and authorize the API v4.

    ...
        "message": "Google Analytics Reporting API has not been used in project xxxxxx-xxxxxx-000000 before or it is disabled. Enable it by visiting https://console.developers.google.com/apis/api/analyticsreporting.googleapis.com/overview?project=xxxxxx-xxxxxx-000000 then retry. If you enabled this API recently, wait a few minutes for the action to propagate to our systems and retry.",
        "domain": "global",
        "reason": "forbidden"
    }
    ],
    "status": "PERMISSION_DENIED"

# example

Call the service :

    $analyticsService = $this->get('media_figaro_analytics.api');
    
    $analytics = $analyticsService->getAnalytics();
    
    $viewId = '000000000'; // set your view id
    
    // get some metrics (last 30 days, date format is yyyy-mm-dd)
    
    $sessions = $analyticsService->getSessionsDateRange($viewId,'30daysAgo','today');
    $bounceRate = $analyticsService->getBounceRateDateRange($viewId,'30daysAgo','today');
    $avgTimeOnPage = $analyticsService->getAvgTimeOnPageDateRange($viewId,'30daysAgo','today');
    $pageViewsPerSession = $analyticsService->getPageviewsPerSessionDateRange($viewId,'30daysAgo','today');
    $percentNewVisits = $analyticsService->getPercentNewVisitsDateRange($viewId,'30daysAgo','today');
    $pageViews = $analyticsService->getPageViewsDateRange($viewId,'30daysAgo','today');
    $avgPageLoadTime = $analyticsService->getAvgPageLoadTimeDateRange($viewId,'30daysAgo','today');

# more tools

Try the Symfony Debug Toolbar Git : https://github.com/kendrick-k/symfony-debug-toolbar-git
