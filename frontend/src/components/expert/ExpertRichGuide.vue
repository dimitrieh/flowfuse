<template>
    <div class="ff-expert-rich-guide">
        <!-- Setup Guide Badge -->
        <div class="guide-badge">
            <span>Setup Guide</span>
        </div>

        <!-- Title and Summary -->
        <div class="guide-header">
            <h3 class="guide-title">{{ guide.title }}</h3>
            <p v-if="guide.summary" class="guide-summary">{{ guide.summary }}</p>
        </div>

        <!-- Steps Section -->
        <div v-if="guide.steps && guide.steps.length > 0" class="guide-steps">
            <h4 class="section-title">Steps:</h4>
            <ol class="steps-list">
                <li v-for="(step, index) in guide.steps" :key="index" class="step-item">
                    <div class="step-number">{{ index + 1 }}</div>
                    <div class="step-content">
                        <h5 class="step-title">{{ step.title }}</h5>
                        <p class="step-detail">{{ step.detail }}</p>
                    </div>
                </li>
            </ol>
        </div>

        <section class="flex flex-col gap-4">
            <!-- Node Packages Section -->
            <div v-if="guide.nodePackages && guide.nodePackages.length > 0" class="guide-packages">
                <h4 class="section-title">Required Node Packages</h4>
                <div class="packages-grid">
                    <PackageResourceCard v-for="(pkg, index) in guide.nodePackages" :key="index" :nodePackage="pkg" />
                </div>
            </div>

            <!-- Resources Section -->
            <div v-if="guide.resources && guide.resources.length > 0" class="guide-resources">
                <h4 class="section-title">Related Resources</h4>
                <div class="resources-grid">
                    <StandardResourceCard v-for="(resource, index) in guide.resources" :key="index" :resource="resource" />
                </div>
            </div>

            <!-- Resources Section -->
            <div v-if="guide.flows && guide.flows.length > 0" class="guide-flows">
                <h4 class="section-title">Related Flows</h4>
                <div class="resources-grid">
                    <FlowResourceCard v-for="(flow, index) in guide.flows" :key="index" :flow="flow" />
                </div>
            </div>
        </section>
    </div>
</template>

<script>
import FlowResourceCard from './resource-cards/FlowResourceCard.vue'
import PackageResourceCard from './resource-cards/PackageResourceCard.vue'
import StandardResourceCard from './resource-cards/StandardResourceCard.vue'

export default {
    name: 'ExpertRichGuide',
    components: { StandardResourceCard, PackageResourceCard, FlowResourceCard },
    props: {
        message: {
            type: Object,
            required: true,
            validator: (message) => {
                return message.guide?.title !== undefined
            }
        }
    },
    computed: {
        guide () {
            return this.message.guide
        }
    }
}
</script>

<style scoped>
.ff-expert-rich-guide {
    display: flex;
    flex-direction: column;
    gap: 0;
}

.guide-badge {
    display: inline-flex;
    align-self: flex-start;
    margin-bottom: 0.75rem; /* mb-3 */
}

.guide-badge span {
    display: inline-block;
    padding: 0.5rem 0.75rem; /* py-2 px-3 */
    background-color: var(--color-indigo-100);
    color: var(--color-indigo-700);
    font-size: 0.875rem; /* text-sm */
    border-radius: 9999px; /* rounded-full */
}

.guide-header .guide-title {
    font-size: 1.125rem; /* text-lg */
    font-weight: 600; /* font-semibold */
    color: #111827; /* text-gray-900 */
    margin: 0 0 0.5rem 0; /* mb-2 */
}

.guide-header .guide-summary {
    color: #374151; /* text-gray-700 */
    margin: 0 0 1rem 0; /* mb-4 */
    line-height: 1.625;
}

.section-title {
    font-size: 1rem; /* text-base */
    font-weight: 500; /* font-medium */
    color: #111827; /* text-gray-900 */
    margin: 0 0 0.75rem 0; /* mb-3 */
}

.guide-steps {
    margin-bottom: 1rem; /* mb-4 */
}

.guide-steps .steps-list {
    list-style: none;
    padding: 0;
    margin: 0;
    display: flex;
    flex-direction: column;
    gap: 0.75rem; /* space-y-3 */
}

.guide-steps .step-item {
    display: flex;
    align-items: flex-start;
}

.guide-steps .step-number {
    flex-shrink: 0;
    width: 1.5rem; /* w-6 */
    height: 1.5rem; /* h-6 */
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: var(--color-indigo-600);
    color: white;
    font-size: 0.875rem; /* text-sm */
    border-radius: 50%; /* rounded-full */
    margin-right: 0.75rem; /* mr-3 */
    margin-top: 0.125rem; /* mt-0.5 */
}

.guide-steps .step-content {
    flex: 1;
}

.guide-steps .step-content .step-title {
    font-size: 1rem;
    font-weight: 500;
    color: var(--color-gray-900);
    margin: 0 0 0.25rem 0;
}

.guide-steps .step-content .step-detail {
    font-size: 0.875rem;
    color: var(--color-gray-600);
    margin: 0.25rem 0 0 0;
    line-height: 1.5;
}

.guide-packages .packages-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 0.5rem;
}

.guide-resources .resources-grid {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
}

.guide-flows .resources-grid {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
}
</style>
