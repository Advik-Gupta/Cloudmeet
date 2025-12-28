<script lang="ts">
  interface Props {
    bookingForm: {
      name: string;
      email: string;
      notes: string;
    };
    bookingStatus: "idle" | "submitting" | "success" | "error";
    bookingError: string;
    brandColor: string;
    brandDark: string;

    meetingType: "google_meet" | "teams";
    customMessage: string;
    useCustomMessage: boolean;

    onSubmit: (e: Event) => void;
  }

  let {
    bookingForm = $bindable(),
    bookingStatus,
    bookingError,
    brandColor,
    brandDark,
    meetingType = $bindable(),
    customMessage = $bindable(),
    useCustomMessage = $bindable(),
    onSubmit,
  }: Props = $props();
</script>

<div class="max-w-md">
  <h2 class="text-xl font-semibold text-gray-900 mb-6">Enter Details</h2>

  {#if bookingError}
    <div
      class="bg-red-50 border border-red-200 text-red-800 rounded-lg p-4 mb-6"
    >
      {bookingError}
    </div>
  {/if}

  <form onsubmit={onSubmit} class="space-y-5">
    <!-- Name -->
    <div>
      <label for="name" class="block text-sm font-medium text-gray-700 mb-2">
        Name *
      </label>
      <input
        type="text"
        id="name"
        bind:value={bookingForm.name}
        required
        class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:border-transparent outline-none"
        style="--tw-ring-color: {brandColor}"
      />
    </div>

    <!-- Email -->
    <div>
      <label for="email" class="block text-sm font-medium text-gray-700 mb-2">
        Email *
      </label>
      <input
        type="email"
        id="email"
        bind:value={bookingForm.email}
        required
        class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:border-transparent outline-none"
        style="--tw-ring-color: {brandColor}"
      />
    </div>

    <!-- Custom Message Toggle -->
    <div
      class="flex items-center justify-between rounded-lg border border-gray-200 px-4 py-3"
    >
      <div>
        <p class="text-sm font-medium text-gray-900">
          Add custom message / link
        </p>
        <p class="text-xs text-gray-500">
          Optional instructions for the attendee
        </p>
      </div>

      <button
        type="button"
        role="switch"
        aria-checked={useCustomMessage}
        aria-label="Toggle custom message"
        onclick={() => {
          useCustomMessage = !useCustomMessage;
          if (!useCustomMessage) customMessage = "";
        }}
        class="relative inline-flex h-6 w-11 items-center rounded-full transition
      {useCustomMessage ? 'bg-blue-600' : 'bg-gray-300'}"
        style={useCustomMessage ? `background-color: var(--brand-color)` : ""}
      >
        <span
          class="inline-block h-4 w-4 transform rounded-full bg-white transition
        {useCustomMessage ? 'translate-x-6' : 'translate-x-1'}"
        ></span>
      </button>
    </div>

    <!-- Custom Message -->
    {#if useCustomMessage}
      <div>
        <label class="block text-sm font-medium text-gray-700 mb-2">
          Meeting instructions *
        </label>
        <textarea
          bind:value={customMessage}
          required
          rows="4"
          class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:border-transparent outline-none resize-none"
          style="--tw-ring-color: {brandColor}"
          placeholder="e.g. Zoom link, phone number, physical address, etc."
        ></textarea>
      </div>
    {/if}

    <!-- Notes -->
    <div>
      <label for="notes" class="block text-sm font-medium text-gray-700 mb-2">
        Please share anything that will help prepare for our meeting.
      </label>
      <textarea
        id="notes"
        bind:value={bookingForm.notes}
        rows="4"
        class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:border-transparent outline-none resize-none"
        style="--tw-ring-color: {brandColor}"
      ></textarea>
    </div>

    <!-- Submit -->
    <button
      type="submit"
      disabled={bookingStatus === "submitting"}
      class="w-full text-white py-3 px-6 rounded-full font-semibold transition disabled:opacity-50"
      style="background-color: {brandColor}"
      onmouseenter={(e) => (e.currentTarget.style.backgroundColor = brandDark)}
      onmouseleave={(e) => (e.currentTarget.style.backgroundColor = brandColor)}
    >
      {bookingStatus === "submitting" ? "Scheduling..." : "Schedule Event"}
    </button>
  </form>
</div>
