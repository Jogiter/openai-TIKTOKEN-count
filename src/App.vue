<template>
  <div class="flex flex-col items-center justify-start min-h-screen mt-10">
    <h2 class="uppercase text-lg font-bold mb-4">count tokens with tiktoken</h2>
    <div class="w-full max-w-md overflow-hidden rounded-lg shadow-xs mb-4">
      <div class="w-full overflow-x-auto">
        <table class="w-full whitespace-no-wrap">
          <thead>
            <tr
              class="
                text-xs
                font-semibold
                tracking-wide
                text-left text-gray-500
                uppercase
                border-b
                bg-gray-50
              "
            >
              <th class="px-4 py-3">Encoding Name</th>
              <th class="px-4 py-3">OpenAI Models</th>
            </tr>
          </thead>
          <tbody class="bg-white divide-y">
            <tr class="text-gray-700">
              <td class="px-4 py-3">cl100k_base</td>
              <td class="px-4 py-3">
                gpt-4, gpt-3.5-turbo, text-embedding-ada-002
              </td>
            </tr>
            <tr class="text-gray-700">
              <td class="px-4 py-3">p50k_base</td>
              <td class="px-4 py-3">
                Codex models, text-davinci-002, text-davinci-003
              </td>
            </tr>
            <tr class="text-gray-700">
              <td class="px-4 py-3">r50k_base (or gpt2)</td>
              <td class="px-4 py-3">GPT-3 models like davinci</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <form @submit.prevent="handleSubmit" class="w-full max-w-md">
      <div class="mb-4">
        <select
          v-model="selectedOption"
          class="form-select w-full h-10 p-2 border border-gray-300"
        >
          <option value="gpt2">gpt2</option>
          <option value="r50k_base">r50k_base</option>
          <option value="p50k_base">p50k_base</option>
          <option value="p50k_edit">p50k_edit</option>
          <option value="cl100k_base">cl100k_base</option>
        </select>
      </div>
      <div class="mb-4">
        <textarea
          v-model.trim="text"
          rows="10"
          class="form-textarea w-full p-2 border border-gray-300"
          placeholder="Enter text..."
        ></textarea>
      </div>
      <div class="mb-4">
        <button
          type="submit"
          class="
            w-full
            px-4
            py-2
            text-white
            bg-blue-500
            rounded
            hover:bg-blue-700
          "
        >
          Submit
        </button>
      </div>
    </form>
    <div v-if="tokens" class="mt-4">
      <p>Total tokens is {{ tokens }}</p>
    </div>
  </div>
</template>

<script>
import { getEncoding, encodingForModel } from 'js-tiktoken';

export default {
  data() {
    return {
      text: 'Hello world\n\
New lines\n\
Spaces👩‍👦‍👦 👩‍👧‍👦 👩‍👧‍👧 👩‍👩‍👦 👩‍👩‍👧 🇨🇿 Emojis:\
🧑🏾‍💻️🧑🏿‍🎓️🧑🏿‍🏭️🧑🏿‍💻️\n是美國一個人工智能研究實驗室 由非營利組織OpenAI Inc',
      tokens: null,
      selectedOption: 'cl100k_base',
    };
  },
  methods: {
    handleSubmit() {
      const encoding = getEncoding(this.selectedOption);
      this.tokens = encoding.encode(this.text).length;
    },
  },
};
</script>
