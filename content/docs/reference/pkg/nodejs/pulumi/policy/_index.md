---
title: "Package @pulumi/policy"
title_tag: "Package @pulumi/policy | Node.js SDK"
linktitle: "@pulumi/policy"
meta_desc: "Explore members of the @pulumi/policy package."
git_sha: "ce4664886dbb0c41c7f2761b86e813df0feb0265"
---

<!-- WARNING: this page was generated by a tool. Do not edit it by hand. -->
<!-- To change it, please see https://github.com/pulumi/docs/tree/master/tools/tscdocgen. -->

{{< langchoose nodeonly >}}

```javascript
var policy = require("@pulumi/policy");
```

```typescript
import * as policy from "@pulumi/policy";
```






<h3>APIs</h3>
<ul class="api">
    <li><a href="#EnforcementLevel"><span class="symbol api"></span>EnforcementLevel</a></li>
    <li><a href="#Policies"><span class="symbol api"></span>Policies</a></li>
    <li><a href="#Policy"><span class="symbol api"></span>Policy</a></li>
    <li><a href="#PolicyPack"><span class="symbol api"></span>PolicyPack</a></li>
    <li><a href="#PolicyPackArgs"><span class="symbol api"></span>PolicyPackArgs</a></li>
    <li><a href="#PolicyResource"><span class="symbol api"></span>PolicyResource</a></li>
    <li><a href="#ReportViolation"><span class="symbol api"></span>ReportViolation</a></li>
    <li><a href="#ResourceValidation"><span class="symbol api"></span>ResourceValidation</a></li>
    <li><a href="#ResourceValidationArgs"><span class="symbol api"></span>ResourceValidationArgs</a></li>
    <li><a href="#ResourceValidationPolicy"><span class="symbol api"></span>ResourceValidationPolicy</a></li>
    <li><a href="#StackValidation"><span class="symbol api"></span>StackValidation</a></li>
    <li><a href="#StackValidationArgs"><span class="symbol api"></span>StackValidationArgs</a></li>
    <li><a href="#StackValidationPolicy"><span class="symbol api"></span>StackValidationPolicy</a></li>
    <li><a href="#validateResourceOfType"><span class="symbol api"></span>validateResourceOfType</a></li>
    <li><a href="#validateStackResourcesOfType"><span class="symbol api"></span>validateStackResourcesOfType</a></li>
    <li><a href="#validateTypedResource"><span class="symbol api"></span>validateTypedResource</a></li>
</ul>




<h2 id="apis">APIs</h2>
<h3 class="pdoc-module-header" id="EnforcementLevel" data-link-title="EnforcementLevel">
    <a href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L67">
        type <strong>EnforcementLevel</strong>
    </a>
</h3>

<pre class="highlight"><code><span class='kd'>type</span> EnforcementLevel = <span class='s2'>"advisory"</span> | <span class='s2'>"mandatory"</span> | <span class='s2'>"disabled"</span>;</code></pre>

Indicates the impact of a policy violation.

<h3 class="pdoc-module-header" id="Policies" data-link-title="Policies">
    <a href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L94">
        type <strong>Policies</strong>
    </a>
</h3>

<pre class="highlight"><code><span class='kd'>type</span> Policies = <a href='#ResourceValidationPolicy'>ResourceValidationPolicy</a> | <a href='#StackValidationPolicy'>StackValidationPolicy</a>[];</code></pre>

An array of Policies.

<h3 class="pdoc-module-header" id="Policy" data-link-title="Policy">
    <a href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L74">
        interface <strong>Policy</strong>
    </a>
</h3>

<pre class="highlight"><code><span class='kr'>interface</span> <span class='nx'>Policy</span></code></pre>

A policy function that returns true if a resource definition violates some policy (e.g., "no
public S3 buckets"), and a set of metadata useful for generating helpful messages when the policy
is violated.

<h4 class="pdoc-member-header" id="Policy-description">
<a class="pdoc-child-name" href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L82">property <b>description</b></a>
</h4>

<pre class="highlight"><code><span class='kd'></span>description: <span class='kd'><a href='https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String'>string</a></span>;</code></pre>

A brief description of the policy rule. e.g., "S3 buckets should have default encryption
enabled."

<h4 class="pdoc-member-header" id="Policy-enforcementLevel">
<a class="pdoc-child-name" href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L88">property <b>enforcementLevel</b></a>
</h4>

<pre class="highlight"><code><span class='kd'></span>enforcementLevel: <a href='#EnforcementLevel'>EnforcementLevel</a>;</code></pre>

Indicates what to do on policy violation, e.g., block deployment but allow override with
proper permissions.

<h4 class="pdoc-member-header" id="Policy-name">
<a class="pdoc-child-name" href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L76">property <b>name</b></a>
</h4>

<pre class="highlight"><code><span class='kd'></span>name: <span class='kd'><a href='https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String'>string</a></span>;</code></pre>

An ID for the policy. Must be unique within the current policy set.

<h3 class="pdoc-module-header" id="PolicyPack" data-link-title="PolicyPack">
    <a href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L52">
        class <strong>PolicyPack</strong>
    </a>
</h3>

<pre class="highlight"><code><span class='kr'>class</span> <span class='nx'>PolicyPack</span></code></pre>

A PolicyPack contains one or more policies to enforce.

For example:

```typescript
import * as aws from "@pulumi/aws";
import { PolicyPack, validateResourceOfType } from "@pulumi/policy";

new PolicyPack("aws-typescript", {
    policies: [{
        name: "s3-no-public-read",
        description: "Prohibits setting the publicRead or publicReadWrite permission on AWS S3 buckets.",
        enforcementLevel: "mandatory",
        validateResource: validateResourceOfType(aws.s3.Bucket, (bucket, args, reportViolation) => {
            if (bucket.acl === "public-read" || bucket.acl === "public-read-write") {
                reportViolation("You cannot set public-read or public-read-write on an S3 bucket.");
            }
        }),
    }],
});
```

<h4 class="pdoc-member-header" id="PolicyPack-constructor">
<a class="pdoc-child-name" href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L53"> <b>constructor</b></a>
</h4>


<pre class="highlight"><code><span class='kd'></span><span class='kd'>new</span> PolicyPack(name: <span class='kd'><a href='https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String'>string</a></span>, args: <a href='#PolicyPackArgs'>PolicyPackArgs</a>)</code></pre>

<h3 class="pdoc-module-header" id="PolicyPackArgs" data-link-title="PolicyPackArgs">
    <a href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L22">
        interface <strong>PolicyPackArgs</strong>
    </a>
</h3>

<pre class="highlight"><code><span class='kr'>interface</span> <span class='nx'>PolicyPackArgs</span></code></pre>

The set of arguments for constructing a PolicyPack.

<h4 class="pdoc-member-header" id="PolicyPackArgs-policies">
<a class="pdoc-child-name" href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L26">property <b>policies</b></a>
</h4>

<pre class="highlight"><code><span class='kd'></span>policies: <a href='#Policies'>Policies</a>;</code></pre>

The policies associated with a PolicyPack.

<h3 class="pdoc-module-header" id="PolicyResource" data-link-title="PolicyResource">
    <a href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L280">
        interface <strong>PolicyResource</strong>
    </a>
</h3>

<pre class="highlight"><code><span class='kr'>interface</span> <span class='nx'>PolicyResource</span></code></pre>

PolicyResource represents a resource in the stack.

<h4 class="pdoc-member-header" id="PolicyResource-asType">
<a class="pdoc-child-name" href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L337">method <b>asType</b></a>
</h4>


<pre class="highlight"><code><span class='kd'></span>asType&lt;TResource&gt;(resourceClass: {
    constructor: ;
}): q.ResolvedResource&lt;TResource&gt; | <span class='kd'><a href='https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined'>undefined</a></span></code></pre>


Returns the resource if the type of this resource is the same as `resourceClass`,
otherwise `undefined`.

For example:

```typescript
const buckets = args.resources
    .map(r = r.asType(aws.s3.Bucket))
    .filter(b => b);
for (const bucket of buckets) {
    // ...
}
```

<h4 class="pdoc-member-header" id="PolicyResource-isType">
<a class="pdoc-child-name" href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L318">method <b>isType</b></a>
</h4>


<pre class="highlight"><code><span class='kd'></span>isType&lt;TResource&gt;(resourceClass: {
    constructor: ;
}): <span class='kd'><a href='https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean'>boolean</a></span></code></pre>


Returns true if the type of this resource is the same as `resourceClass`.

For example:

```typescript
for (const resource of args.resources) {
    if (resource.isType(aws.s3.Bucket)) {
        // ...
    }
}
```

<h4 class="pdoc-member-header" id="PolicyResource-name">
<a class="pdoc-child-name" href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L299">property <b>name</b></a>
</h4>

<pre class="highlight"><code><span class='kd'></span>name: <span class='kd'><a href='https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String'>string</a></span>;</code></pre>

The name of the resource.

<h4 class="pdoc-member-header" id="PolicyResource-props">
<a class="pdoc-child-name" href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L289">property <b>props</b></a>
</h4>

<pre class="highlight"><code><span class='kd'></span>props: Record&lt;<span class='kd'><a href='https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String'>string</a></span>, <span class='kd'><a href='https://www.typescriptlang.org/docs/handbook/basic-types.html#any'>any</a></span>&gt;;</code></pre>

The outputs of the resource.

<h4 class="pdoc-member-header" id="PolicyResource-type">
<a class="pdoc-child-name" href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L284">property <b>type</b></a>
</h4>

<pre class="highlight"><code><span class='kd'></span>type: <span class='kd'><a href='https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String'>string</a></span>;</code></pre>

The type of the resource.

<h4 class="pdoc-member-header" id="PolicyResource-urn">
<a class="pdoc-child-name" href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L294">property <b>urn</b></a>
</h4>

<pre class="highlight"><code><span class='kd'></span>urn: <span class='kd'><a href='https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String'>string</a></span>;</code></pre>

The URN of the resource.

<h3 class="pdoc-module-header" id="ReportViolation" data-link-title="ReportViolation">
    <a href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L379">
        type <strong>ReportViolation</strong>
    </a>
</h3>

<pre class="highlight"><code><span class='kd'>type</span> ReportViolation = (message: <span class='kd'><a href='https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String'>string</a></span>, urn?: <span class='kd'><a href='https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined'>undefined</a></span> | <span class='kd'><a href='https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String'>string</a></span>) => <span class='kd'><a href='https://www.typescriptlang.org/docs/handbook/basic-types.html#void'>void</a></span>;</code></pre>

ReportViolation is the callback signature used to report policy violations.

<h3 class="pdoc-module-header" id="ResourceValidation" data-link-title="ResourceValidation">
    <a href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L134">
        type <strong>ResourceValidation</strong>
    </a>
</h3>

<pre class="highlight"><code><span class='kd'>type</span> ResourceValidation = (args: <a href='#ResourceValidationArgs'>ResourceValidationArgs</a>, reportViolation: <a href='#ReportViolation'>ReportViolation</a>) => <a href='https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise'>Promise</a>&lt;<span class='kd'><a href='https://www.typescriptlang.org/docs/handbook/basic-types.html#void'>void</a></span>&gt; | <span class='kd'><a href='https://www.typescriptlang.org/docs/handbook/basic-types.html#void'>void</a></span>;</code></pre>

ResourceValidation is the callback signature for a `ResourceValidationPolicy`. A resource validation
is passed `args` with more information about the resource and a `reportViolation` callback that can be
used to report a policy violation. `reportViolation` can be called multiple times to report multiple
violations against the same resource. `reportViolation` must be passed a message about the violation.
The `reportViolation` signature accepts an optional `urn` argument, which is ignored when validating
resources (the `urn` of the resource being validated is always used).

<h3 class="pdoc-module-header" id="ResourceValidationArgs" data-link-title="ResourceValidationArgs">
    <a href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L139">
        interface <strong>ResourceValidationArgs</strong>
    </a>
</h3>

<pre class="highlight"><code><span class='kr'>interface</span> <span class='nx'>ResourceValidationArgs</span></code></pre>

ResourceValidationArgs is the argument bag passed to a resource validation.

<h4 class="pdoc-member-header" id="ResourceValidationArgs-asType">
<a class="pdoc-child-name" href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L192">method <b>asType</b></a>
</h4>


<pre class="highlight"><code><span class='kd'></span>asType&lt;TResource,TArgs&gt;(resourceClass: {
    constructor: ;
}): Unwrap&lt;NonNullable&lt;TArgs&gt;&gt; | <span class='kd'><a href='https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined'>undefined</a></span></code></pre>


Returns the resource args for `resourceClass` if the type of this resource is the same as `resourceClass`,
otherwise `undefined`.

For example:

```typescript
const bucketArgs = args.AsType(aws.s3.Bucket);
if (bucketArgs) {
    // ...
}
```

<h4 class="pdoc-member-header" id="ResourceValidationArgs-isType">
<a class="pdoc-child-name" href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L175">method <b>isType</b></a>
</h4>


<pre class="highlight"><code><span class='kd'></span>isType&lt;TResource&gt;(resourceClass: {
    constructor: ;
}): <span class='kd'><a href='https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean'>boolean</a></span></code></pre>


Returns true if the type of this resource is the same as `resourceClass`.

For example:

```typescript
if (args.isType(aws.s3.Bucket)) {
    // ...
}
```

<h4 class="pdoc-member-header" id="ResourceValidationArgs-name">
<a class="pdoc-child-name" href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L158">property <b>name</b></a>
</h4>

<pre class="highlight"><code><span class='kd'></span>name: <span class='kd'><a href='https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String'>string</a></span>;</code></pre>

The name of the resource.

<h4 class="pdoc-member-header" id="ResourceValidationArgs-props">
<a class="pdoc-child-name" href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L148">property <b>props</b></a>
</h4>

<pre class="highlight"><code><span class='kd'></span>props: Record&lt;<span class='kd'><a href='https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String'>string</a></span>, <span class='kd'><a href='https://www.typescriptlang.org/docs/handbook/basic-types.html#any'>any</a></span>&gt;;</code></pre>

The properties of the resource.

<h4 class="pdoc-member-header" id="ResourceValidationArgs-type">
<a class="pdoc-child-name" href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L143">property <b>type</b></a>
</h4>

<pre class="highlight"><code><span class='kd'></span>type: <span class='kd'><a href='https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String'>string</a></span>;</code></pre>

The type of the resource.

<h4 class="pdoc-member-header" id="ResourceValidationArgs-urn">
<a class="pdoc-child-name" href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L153">property <b>urn</b></a>
</h4>

<pre class="highlight"><code><span class='kd'></span>urn: <span class='kd'><a href='https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String'>string</a></span>;</code></pre>

The URN of the resource.

<h3 class="pdoc-module-header" id="ResourceValidationPolicy" data-link-title="ResourceValidationPolicy">
    <a href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L117">
        interface <strong>ResourceValidationPolicy</strong>
    </a>
</h3>

<pre class="highlight"><code><span class='kr'>interface</span> <span class='nx'>ResourceValidationPolicy</span> <span class='kr'>extends</span> <a href='#Policy'>Policy</a></code></pre>

ResourceValidationPolicy is a policy that validates a resource definition.

For example:

```typescript
import * as aws from "@pulumi/aws";
import { validateResourceOfType } from "@pulumi/policy";

const s3NoPublicReadPolicy: ResourceValidationPolicy = {
    name: "s3-no-public-read",
    description: "Prohibits setting the publicRead or publicReadWrite permission on AWS S3 buckets.",
    enforcementLevel: "mandatory",
    validateResource: validateResourceOfType(aws.s3.Bucket, (bucket, args, reportViolation) => {
        if (bucket.acl === "public-read" || bucket.acl === "public-read-write") {
            reportViolation("You cannot set public-read or public-read-write on an S3 bucket.");
        }
    }),
};
```

<h4 class="pdoc-member-header" id="ResourceValidationPolicy-description">
<a class="pdoc-child-name" href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L82">property <b>description</b></a>
</h4>

<pre class="highlight"><code><span class='kd'></span>description: <span class='kd'><a href='https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String'>string</a></span>;</code></pre>

A brief description of the policy rule. e.g., "S3 buckets should have default encryption
enabled."

<h4 class="pdoc-member-header" id="ResourceValidationPolicy-enforcementLevel">
<a class="pdoc-child-name" href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L88">property <b>enforcementLevel</b></a>
</h4>

<pre class="highlight"><code><span class='kd'></span>enforcementLevel: <a href='#EnforcementLevel'>EnforcementLevel</a>;</code></pre>

Indicates what to do on policy violation, e.g., block deployment but allow override with
proper permissions.

<h4 class="pdoc-member-header" id="ResourceValidationPolicy-name">
<a class="pdoc-child-name" href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L76">property <b>name</b></a>
</h4>

<pre class="highlight"><code><span class='kd'></span>name: <span class='kd'><a href='https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String'>string</a></span>;</code></pre>

An ID for the policy. Must be unique within the current policy set.

<h4 class="pdoc-member-header" id="ResourceValidationPolicy-validateResource">
<a class="pdoc-child-name" href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L123">property <b>validateResource</b></a>
</h4>

<pre class="highlight"><code><span class='kd'></span>validateResource: <a href='#ResourceValidation'>ResourceValidation</a> | <a href='#ResourceValidation'>ResourceValidation</a>[];</code></pre>

A callback function that validates if a resource definition violates a policy (e.g. "S3 buckets
can't be public"). A single callback function can be specified, or multiple functions, which are
called in order.

<h3 class="pdoc-module-header" id="StackValidation" data-link-title="StackValidation">
    <a href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L265">
        type <strong>StackValidation</strong>
    </a>
</h3>

<pre class="highlight"><code><span class='kd'>type</span> StackValidation = (args: <a href='#StackValidationArgs'>StackValidationArgs</a>, reportViolation: <a href='#ReportViolation'>ReportViolation</a>) => <a href='https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise'>Promise</a>&lt;<span class='kd'><a href='https://www.typescriptlang.org/docs/handbook/basic-types.html#void'>void</a></span>&gt; | <span class='kd'><a href='https://www.typescriptlang.org/docs/handbook/basic-types.html#void'>void</a></span>;</code></pre>

StackValidation is the callback signature for a `StackValidationPolicy`. A stack validation is passed
`args` with more information about the stack and a `reportViolation` callback that can be used to
report a policy violation. `reportViolation` can be called multiple times to report multiple violations
against the stack. `reportViolation` must be passed a message about the violation, and an optional `urn`
to a resource in the stack that's in violation of the policy. Not specifying a `urn` indicates the
overall stack is in violation of the policy.

<h3 class="pdoc-module-header" id="StackValidationArgs" data-link-title="StackValidationArgs">
    <a href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L270">
        interface <strong>StackValidationArgs</strong>
    </a>
</h3>

<pre class="highlight"><code><span class='kr'>interface</span> <span class='nx'>StackValidationArgs</span></code></pre>

StackValidationArgs is the argument bag passed to a resource validation.

<h4 class="pdoc-member-header" id="StackValidationArgs-resources">
<a class="pdoc-child-name" href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L274">property <b>resources</b></a>
</h4>

<pre class="highlight"><code><span class='kd'></span>resources: <a href='#PolicyResource'>PolicyResource</a>[];</code></pre>

The resources in the stack.

<h3 class="pdoc-module-header" id="StackValidationPolicy" data-link-title="StackValidationPolicy">
    <a href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L250">
        interface <strong>StackValidationPolicy</strong>
    </a>
</h3>

<pre class="highlight"><code><span class='kr'>interface</span> <span class='nx'>StackValidationPolicy</span> <span class='kr'>extends</span> <a href='#Policy'>Policy</a></code></pre>

StackValidationPolicy is a policy that validates a stack.

<h4 class="pdoc-member-header" id="StackValidationPolicy-description">
<a class="pdoc-child-name" href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L82">property <b>description</b></a>
</h4>

<pre class="highlight"><code><span class='kd'></span>description: <span class='kd'><a href='https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String'>string</a></span>;</code></pre>

A brief description of the policy rule. e.g., "S3 buckets should have default encryption
enabled."

<h4 class="pdoc-member-header" id="StackValidationPolicy-enforcementLevel">
<a class="pdoc-child-name" href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L88">property <b>enforcementLevel</b></a>
</h4>

<pre class="highlight"><code><span class='kd'></span>enforcementLevel: <a href='#EnforcementLevel'>EnforcementLevel</a>;</code></pre>

Indicates what to do on policy violation, e.g., block deployment but allow override with
proper permissions.

<h4 class="pdoc-member-header" id="StackValidationPolicy-name">
<a class="pdoc-child-name" href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L76">property <b>name</b></a>
</h4>

<pre class="highlight"><code><span class='kd'></span>name: <span class='kd'><a href='https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String'>string</a></span>;</code></pre>

An ID for the policy. Must be unique within the current policy set.

<h4 class="pdoc-member-header" id="StackValidationPolicy-validateStack">
<a class="pdoc-child-name" href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L254">property <b>validateStack</b></a>
</h4>

<pre class="highlight"><code><span class='kd'></span>validateStack: <a href='#StackValidation'>StackValidation</a>;</code></pre>

A callback function that validates if a stack violates a policy.

<h3 class="pdoc-module-header" id="validateResourceOfType" data-link-title="validateResourceOfType">
    <a href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L214">
        function <strong>validateResourceOfType</strong>
    </a>
</h3>


<pre class="highlight"><code><span class='kd'></span>validateResourceOfType&lt;TResource,TArgs&gt;(resourceClass: {
    constructor: ;
}, validate: (props: Unwrap&lt;NonNullable&lt;TArgs&gt;&gt;, args: <a href='#ResourceValidationArgs'>ResourceValidationArgs</a>, reportViolation: <a href='#ReportViolation'>ReportViolation</a>) => <a href='https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise'>Promise</a>&lt;<span class='kd'><a href='https://www.typescriptlang.org/docs/handbook/basic-types.html#void'>void</a></span>&gt; | <span class='kd'><a href='https://www.typescriptlang.org/docs/handbook/basic-types.html#void'>void</a></span>): <a href='#ResourceValidation'>ResourceValidation</a></code></pre>


A helper function that returns a strongly-typed resource validation function, used to check only resources of the
specified resource class.

For example:

```typescript
validateResource: validateResourceOfType(aws.s3.Bucket, (bucket, args, reportViolation) => {
    for (const bucket of buckets) {
        // ...
    }
}),
```

<h3 class="pdoc-module-header" id="validateStackResourcesOfType" data-link-title="validateStackResourcesOfType">
    <a href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L359">
        function <strong>validateStackResourcesOfType</strong>
    </a>
</h3>


<pre class="highlight"><code><span class='kd'></span>validateStackResourcesOfType&lt;TResource&gt;(resourceClass: {
    constructor: ;
}, validate: (resources: q.ResolvedResource&lt;TResource&gt;[], args: <a href='#StackValidationArgs'>StackValidationArgs</a>, reportViolation: <a href='#ReportViolation'>ReportViolation</a>) => <a href='https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise'>Promise</a>&lt;<span class='kd'><a href='https://www.typescriptlang.org/docs/handbook/basic-types.html#void'>void</a></span>&gt; | <span class='kd'><a href='https://www.typescriptlang.org/docs/handbook/basic-types.html#void'>void</a></span>): <a href='#StackValidation'>StackValidation</a></code></pre>


A helper function that returns a strongly-typed stack validation function, used to check only resources of the
specified resource class.

For example:

```typescript
validateStack: validateStackResourcesOfType(aws.s3.Bucket, (buckets, args, reportViolation) => {
    for (const bucket of buckets) {
        // ...
    }
}),
```

<h3 class="pdoc-module-header" id="validateTypedResource" data-link-title="validateTypedResource">
    <a href="https://github.com/pulumi/pulumi-policy/blob/{{< param git_sha >}}/sdk/nodejs/policy/policy.ts#L237">
        function <strong>validateTypedResource</strong>
    </a>
</h3>


<div class="note note-deprecated">
<i class="fas fa-exclamation-triangle pr-2"></i><strong>DEPRECATED</strong>
This is deprecated and will be removed in a future version. Use `validateResourceOfType` instead.
</div>
<pre class="highlight"><code><span class='kd'></span>validateTypedResource&lt;TResource,TArgs&gt;(resourceClass: {
    constructor: ;
}, validate: (props: Unwrap&lt;NonNullable&lt;TArgs&gt;&gt;, args: <a href='#ResourceValidationArgs'>ResourceValidationArgs</a>, reportViolation: <a href='#ReportViolation'>ReportViolation</a>) => <a href='https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise'>Promise</a>&lt;<span class='kd'><a href='https://www.typescriptlang.org/docs/handbook/basic-types.html#void'>void</a></span>&gt; | <span class='kd'><a href='https://www.typescriptlang.org/docs/handbook/basic-types.html#void'>void</a></span>): <a href='#ResourceValidation'>ResourceValidation</a></code></pre>


A helper function that returns a strongly-typed resource validation function, used to check only resources of the
specified resource class.

